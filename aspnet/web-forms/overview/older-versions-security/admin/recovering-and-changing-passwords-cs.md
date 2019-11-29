---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Recupero e modifica delle password (C#) | Microsoft Docs
author: rick-anderson
description: ASP.NET include due controlli Web che consentono di recuperare e modificare le password. Il controllo PasswordRecovery consente a un visitatore di recuperare il suo PA perso...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 8c07b8a3c36e4863c6d2d356b8483544ac4cafeb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576476"
---
# <a name="recovering-and-changing-passwords-c"></a>Recupero e modifica delle password (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET include due controlli Web che consentono di recuperare e modificare le password. Il controllo PasswordRecovery consente a un visitatore di recuperare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la propria password. Analogamente agli altri controlli Web correlati all'accesso che abbiamo visto in questa serie di esercitazioni, i controlli PasswordRecovery e ChangePassword funzionano con il Framework di appartenenza dietro le quinte per reimpostare o modificare le password degli utenti.

## <a name="introduction"></a>Introduzione

Tra i siti Web per la mia banca, l'azienda di utilità, la società telefonica, gli account di posta elettronica e i portali Web personalizzati, I, come la maggior parte delle persone, hanno dozzine di password diverse da ricordare. Con un numero così elevato di credenziali per memorizzare questi giorni, non è insolito che gli utenti dimentichino la propria password. Per tenere conto di questo, i siti Web che offrono account utente devono includere un modo per consentire a un utente di recuperare la propria password. Questo processo in genere comporta la generazione di una nuova password casuale e l'invio di un messaggio di posta elettronica all'indirizzo di posta elettronica dell'utente nel file. Una volta ricevuta la nuova password, la maggior parte degli utenti torna al sito e cambia la password da quella generata in modo casuale a una più memorabile.

ASP.NET include due controlli Web che consentono di recuperare e modificare le password. Il controllo PasswordRecovery consente a un visitatore di recuperare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la propria password. Analogamente agli altri controlli Web correlati all'accesso che abbiamo visto in questa serie di esercitazioni, i controlli PasswordRecovery e ChangePassword funzionano con il Framework di appartenenza dietro le quinte per reimpostare o modificare le password degli utenti.

In questa esercitazione verrà esaminato l'utilizzo di questi due controlli. Si vedrà anche come modificare e reimpostare a livello di codice la password di un utente tramite i metodi `ChangePassword` e `ResetPassword` della classe `MembershipUser`.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Passaggio 1: consentire agli utenti di recuperare le password perse

Tutti i siti Web che supportano gli account utente devono fornire agli utenti un meccanismo per il recupero delle password dimenticate. La novità è che l'implementazione di tale funzionalità in ASP.NET è una brezza grazie al controllo Web PasswordRecovery. Il controllo PasswordRecovery esegue il rendering di un'interfaccia che richiede all'utente il nome utente e, se necessario, la risposta alla domanda di sicurezza. Invia quindi un email all'utente per la password.

> [!NOTE]
> Poiché i messaggi di posta elettronica vengono trasmessi in rete in testo normale, vi sono rischi per la sicurezza di inviare la password di un utente tramite posta elettronica.

Il controllo PasswordRecovery è costituito da tre visualizzazioni:

- **Nome utente** : richiede al visitatore il nome utente. Questa è la visualizzazione iniziale.
- **Domanda**: Visualizza il nome utente e la domanda di sicurezza come testo, insieme a una casella di testo in cui l'utente deve immettere la risposta alla domanda di sicurezza.
- **Operazione completata**: Visualizza un messaggio che informa l'utente che la password è stata inviata tramite posta elettronica.

Le visualizzazioni visualizzate e le azioni eseguite dal controllo PasswordRecovery dipendono dalle impostazioni di configurazione di appartenenza seguenti:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

L'impostazione `RequiresQuestionAndAnswer` del Framework di appartenenza indica se gli utenti devono specificare una domanda e una risposta di sicurezza per la registrazione per un account. Come illustrato nell'esercitazione <a id="_msoanchor_1"> </a> [*creazione di account utente*](../membership/creating-user-accounts-cs.md) , se `RequiresQuestionAndAnswer` è true (impostazione predefinita), l'interfaccia di CreateUserWizard include i controlli TextBox per la domanda e la risposta di sicurezza del nuovo utente. Se `RequiresQuestionAndAnswer` è false, non viene raccolta alcuna informazione di questo tipo. Analogamente, se `RequiresQuestionAndAnswer` è true, il controllo PasswordRecovery Visualizza la visualizzazione delle domande dopo che l'utente ha immesso il nome utente. la password viene recuperata solo se l'utente immette la risposta di sicurezza corretta. Se `RequiresQuestionAndAnswer` è false, tuttavia, il controllo PasswordRecovery si sposta direttamente dalla visualizzazione nome utente alla visualizzazione Operazione riuscita.

Dopo che l'utente ha fornito il nome utente o il nome utente e la risposta di sicurezza, se `RequiresQuestionAndAnswer` è true, PasswordRecovery invia all'utente la password. Se l'opzione `EnablePasswordRetrieval` è impostata su true, all'utente viene inviato un messaggio di posta elettronica con la password corrente. Se è impostato su false e `EnablePasswordReset` è impostato su true, il controllo PasswordRecovery genera una nuova password casuale per l'utente e invia ai messaggi di posta elettronica la nuova password. Se sia `EnablePasswordRetrieval` che `EnablePasswordReset` sono false, il controllo PasswordRecovery genera un'eccezione.

> [!NOTE]
> Ricordare che il `SqlMembershipProvider` archivia le password degli utenti in uno dei tre formati seguenti: Clear, Hashed (impostazione predefinita) o Encrypted. Il meccanismo di archiviazione utilizzato dipende dalle impostazioni di configurazione dell'appartenenza. l'applicazione demo usa il formato di password con hash. Quando si utilizza il formato con hash della password, l'opzione `EnablePasswordRetrieval` deve essere impostata su false perché il sistema non è in grado di determinare la password effettiva dell'utente dalla versione con hash archiviata nel database.

Nella figura 1 viene illustrato il modo in cui l'interfaccia e il comportamento di PasswordRecovery sono influenzati dalla configurazione dell'appartenenza.

[![RequiresQuestionAndAnswer, EnablePasswordRetrieval e EnablePasswordReset influenzano l'aspetto e il comportamento del controllo PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Figura 1**: le `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`e `EnablePasswordReset` influenzano l'aspetto e il comportamento del controllo PasswordRecovery ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image3.png))

> [!NOTE]
> <a id="_msoanchor_2"> </a>Nell'esercitazione [*creazione dello schema di appartenenza in SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) è stato configurato il provider di appartenenze impostando `RequiresQuestionAndAnswer` su true, `EnablePasswordRetrieval` su false e `EnablePasswordReset` su true.

### <a name="using-the-passwordrecovery-control"></a>Uso del controllo PasswordRecovery

Esaminiamo ora l'uso del controllo PasswordRecovery in una pagina ASP.NET. Aprire `RecoverPassword.aspx` e trascinare un controllo PasswordRecovery dalla casella degli strumenti nella finestra di progettazione; impostare la relativa `ID` su `RecoverPwd`. Analogamente ai controlli Web login e CreateUserWizard, le visualizzazioni del controllo PasswordRecovery eseguono il rendering di un'interfaccia composita avanzata che include etichette, caselle di testo, pulsanti e controlli di convalida. È possibile personalizzare l'aspetto delle visualizzazioni tramite le proprietà di stile del controllo o convertendo le visualizzazioni in modelli. Lo lascio come esercizio per il lettore interessato.

Quando un utente visita la pagina, immetterà il proprio nome utente e fare clic sul pulsante Invia. Poiché la proprietà `RequiresQuestionAndAnswer` è stata impostata su true nelle impostazioni di configurazione dell'appartenenza, il controllo PasswordRecovery visualizzerà quindi la visualizzazione domanda. Dopo che l'utente immette la risposta di sicurezza corretta e fa clic su Invia, il controllo PasswordRecovery aggiornerà la password dell'utente a una generata in modo casuale e invierà la password all'indirizzo di posta elettronica nel file. Tutto ciò era possibile senza dover scrivere una sola riga di codice.

Prima di eseguire il test di questa pagina, è necessario specificare le impostazioni per il recapito della posta in `Web.config`. Il controllo PasswordRecovery si basa su queste impostazioni per l'invio del messaggio di posta elettronica.

La configurazione per il recapito tramite posta elettronica viene specificata tramite l' [elemento`<mailSettings>`](https://msdn.microsoft.com/library/w355a94k.aspx)dell' [elemento`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx). Utilizzare l' [elemento`<smtp>`](https://msdn.microsoft.com/library/ms164240.aspx) per indicare il metodo di recapito e l'indirizzo predefinito da. Il markup seguente consente di configurare le impostazioni di posta elettronica per l'utilizzo di un server SMTP di rete denominato `smtp.example.com` sulla porta 25 e con le credenziali nome utente e password del nome utente e della password.

> [!NOTE]
> `<system.net>` è un elemento figlio dell'elemento `<configuration>` radice e un elemento di pari livello di `<system.web>`. Pertanto, non inserire l'elemento `<system.net>` all'interno dell'elemento `<system.web>`; è invece necessario inserirlo nello stesso livello.

[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Oltre a utilizzare un server SMTP nella rete, è possibile specificare una directory di prelievo in cui devono essere inseriti i messaggi di posta elettronica da inviare.

Una volta configurate le impostazioni SMTP, visitare la pagina `RecoverPassword.aspx` tramite un browser. Per prima cosa, provare a immettere un nome utente che non esiste nell'archivio utente. Come illustrato nella figura 2, il controllo PasswordRecovery Visualizza un messaggio che indica che non è possibile accedere alle informazioni utente. Il testo del messaggio può essere personalizzato tramite la [proprietà`UserNameFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)del controllo.

[![viene visualizzato un messaggio di errore se viene immesso un nome utente non valido](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Figura 2**: viene visualizzato un messaggio di errore se viene immesso un nome utente non valido ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image6.png))

Immettere ora un nome utente. Usare il nome utente di un account del sistema con un indirizzo di posta elettronica a cui è possibile accedere e di cui si conosce la risposta di sicurezza. Dopo aver immesso il nome utente e aver fatto clic su Submit, il controllo PasswordRecovery Visualizza la visualizzazione della domanda. Come per la visualizzazione nome utente, se si immette una risposta non corretta, il controllo PasswordRecovery Visualizza un messaggio di errore (vedere la figura 3). Per personalizzare questo messaggio di errore, utilizzare la [proprietà`QuestionFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) .

[![viene visualizzato un messaggio di errore se l'utente immette una risposta di sicurezza non valida](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Figura 3**: viene visualizzato un messaggio di errore se l'utente immette una risposta di sicurezza non valida ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image9.png))

Infine, immettere la risposta di sicurezza corretta e fare clic su Submit (Invia). Dietro le quinte, il controllo PasswordRecovery genera una password casuale, la assegna all'account utente, invia un messaggio di posta elettronica che informa l'utente della nuova password (vedere la figura 4), quindi Visualizza la visualizzazione Operazione riuscita.

[![all'utente viene inviato un messaggio di posta elettronica con la nuova password](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Figura 4**: all'utente viene inviato un messaggio di posta elettronica con la nuova password ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image12.png))

### <a name="customizing-the-email"></a>Personalizzazione del messaggio di posta elettronica

Il messaggio di posta elettronica predefinito inviato dal controllo PasswordRecovery è piuttosto noioso (vedere la figura 4). Il messaggio viene inviato dall'account specificato nell'attributo `from` dell'elemento `<smtp>` con la password dell'oggetto e il corpo del testo normale:

Tornare al sito e accedere usando le informazioni seguenti.

Nome utente: *username*

Password: *password*

Questo messaggio può essere personalizzato a livello di codice tramite un gestore eventi per l' [evento`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)del controllo PasswordRecovery oppure in modo dichiarativo tramite la [Proprietà`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Si analizzeranno entrambe le opzioni.

L'evento `SendingMail` viene generato immediatamente prima dell'invio del messaggio di posta elettronica e rappresenta l'ultima possibilità di modificare a livello di codice il messaggio di posta elettronica. Quando viene generato questo evento, al gestore dell'evento viene passato un oggetto di tipo [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), la cui proprietà `Message` contiene un riferimento al messaggio di posta elettronica che sta per essere inviato.

Creare un gestore eventi per l'evento `SendingMail` e aggiungere il codice seguente, che aggiunge `webmaster@example.com` a livello di codice all'elenco CC.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

Il messaggio di posta elettronica può essere configurato anche tramite mezzi dichiarativi. La proprietà `MailDefinition` di PasswordRecovery è un oggetto di tipo [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). La classe `MailDefinition` offre un host di proprietà correlate alla posta elettronica, tra cui `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`e altro. Per i principianti, impostare la [proprietà`Subject`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) su un elemento più descrittivo rispetto a quello usato per impostazione predefinita (password), ad esempio la password è stata reimpostata...

Per personalizzare il corpo del messaggio di posta elettronica, è necessario creare un file di modello di posta elettronica separato che contenga il contenuto del corpo. Per iniziare, creare una nuova cartella nel sito Web denominato `EmailTemplates`. Aggiungere quindi un nuovo file di testo a questa cartella denominata `PasswordRecovery.txt` e aggiungere il contenuto seguente:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Si noti l'uso dei segnaposto `<%UserName%>` e `<%Password%>`. Il controllo PasswordRecovery sostituisce automaticamente questi due segnaposto con il nome utente e la password recuperati prima di inviare il messaggio di posta elettronica.

Infine, puntare la [proprietà`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) del `MailDefinition`al modello di messaggio di posta elettronica appena creato (`~/EmailTemplates/PasswordRecovery.txt`).

Dopo aver apportato queste modifiche, visitare la pagina `RecoverPassword.aspx` e immettere il nome utente e la risposta di sicurezza. Si riceve un messaggio di posta elettronica simile a quello illustrato nella figura 5. Si noti che `webmaster@example.com` è stato CC e che l'oggetto e il corpo sono stati aggiornati.

[![l'oggetto, il corpo e l'elenco CC sono stati aggiornati](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Figura 5**: l'elenco oggetto, corpo e CC è stato aggiornato ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image15.png))

Per inviare un set di messaggi di posta elettronica in formato HTML [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) true (il valore predefinito è false) e aggiornare il modello di messaggio di posta elettronica per includere HTML.

La proprietà `MailDefinition` non è univoca per la classe PasswordRecovery. Come si vedrà nel passaggio 2, il controllo ChangePassword offre anche una proprietà `MailDefinition`. Inoltre, il controllo CreateUserWizard include tale proprietà che è possibile configurare per inviare automaticamente un messaggio di posta elettronica di benvenuto ai nuovi utenti.

> [!NOTE]
> Attualmente non sono presenti collegamenti nel percorso di navigazione a sinistra per raggiungere la pagina `RecoverPassword.aspx`. Un utente è interessato a visitare questa pagina solo se non è stato in grado di accedere al sito. Aggiornare quindi la pagina `Login.aspx` per includere un collegamento alla pagina `RecoverPassword.aspx`.

### <a name="programmatically-resetting-a-users-password"></a>Reimpostazione della password di un utente a livello di codice

Quando si ripristina la password di un utente, il controllo PasswordRecovery chiama il [metodo`ResetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)dell'oggetto `MembershipUser`. Questo metodo presenta due overload:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** : Reimposta la password di un utente. Utilizzare questo overload se `RequiresQuestionAndAnswer` è false.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** : Reimposta la password di un utente solo se la *securityAnswer* fornita è corretta. Utilizzare questo overload se `RequiresQuestionAndAnswer` è true.

Entrambi gli overload restituiscono la nuova password generata in modo casuale.

Analogamente agli altri metodi del Framework di appartenenza, il `ResetPassword` metodo delega al provider configurato. Il `SqlMembershipProvider` richiama il stored procedure di `aspnet_Membership_ResetPassword`, passando il nome utente dell'utente, la nuova password e la risposta alla password fornita, tra gli altri campi. Il stored procedure garantisce che la risposta alla password corrisponda, quindi aggiorna la password dell'utente.

Alcune note sull'implementazione di basso livello:

- Un utente bloccato non può reimpostare la password. Tuttavia, un utente non approvato può. Gli stati bloccati e approvati verranno illustrati più dettagliatamente nell' <a id="_msoanchor_3"> </a>esercitazione [*sblocco e approvazione*](unlocking-and-approving-user-accounts-cs.md) degli account utente.
- Se la risposta alla password non è corretta, viene incrementato il numero di tentativi di risposta alla password non riusciti dell'utente. Se si verifica un numero specificato di tentativi di risposta di sicurezza non validi entro un intervallo di tempo specificato, l'utente viene bloccato.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Una parola sulla modalità di generazione delle password casuali

Le password generate in modo casuale visualizzate nei messaggi di posta elettronica nelle figure 4 e 5 vengono create dal [metodo`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)della classe di appartenenza. Questo metodo accetta due valori integer parametri- *length* e *numberOfNonAlphanumericCharacters* -e restituisce una stringa di *lunghezza* minima di caratteri con almeno *numberOfNonAlphanumericCharacters* di caratteri non alfanumerici. Quando questo metodo viene chiamato dall'interno delle classi di appartenenza o dei controlli Web correlati all'accesso, i valori per questi due parametri sono determinati dalle proprietà `MinRequiredPasswordLength` e `MinRequiredNonalphanumericCharacters` della configurazione di appartenenza, che vengono impostate rispettivamente su 7 e 1.

Il metodo `GeneratePassword` usa un generatore di numeri casuali crittograficamente sicuro per assicurarsi che non esistano distorsioni nei caratteri casuali selezionati. Inoltre, `GeneratePassword` è `public`, vale a dire che è possibile usarlo direttamente dall'applicazione ASP.NET se è necessario generare stringhe o password casuali.

> [!NOTE]
> La classe `SqlMembershipProvider` genera sempre una password casuale di almeno 14 caratteri, pertanto se `MinRequiredPasswordLength` è inferiore a 14, il valore viene ignorato.

## <a name="step-2-changing-passwords"></a>Passaggio 2: modifica delle password

Le password generate in modo casuale sono difficili da ricordare. Si consideri la password illustrata nella figura 4: `WWGUZv(f2yM:Bd`. Provare a eseguire il commit della memoria. Inutile dire che, dopo che un utente ha inviato una password generata in modo casuale di questo tipo, potrà modificare la password in modo più memorabile.

Utilizzare il controllo ChangePassword per creare un'interfaccia che consentirà a un utente di modificare la password. Analogamente al controllo PasswordRecovery, il controllo ChangePassword è costituito da due visualizzazioni: Change password e Success. La visualizzazione cambia password richiede all'utente le password precedenti e nuove. Quando si specifica la password precedente corretta e una nuova password che soddisfa i requisiti di lunghezza minima e caratteri non alfanumerici, il controllo ChangePassword aggiorna la password dell'utente e visualizza la visualizzazione Operazione riuscita.

> [!NOTE]
> Il controllo ChangePassword modifica la password dell'utente richiamando il [metodo`ChangePassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)dell'oggetto `MembershipUser`. Il metodo ChangePassword accetta due `string` parametri di input, *oldPassword* e *newPassword*, e aggiorna l'account dell'utente con il *newPassword*, supponendo che il *oldPassword* fornito sia corretto.

Aprire la pagina `ChangePassword.aspx` e aggiungere un controllo ChangePassword alla pagina, denominarlo `ChangePwd`. A questo punto, nella visualizzazione progettazione dovrebbe essere visualizzata la visualizzazione cambia password (vedere la figura 6). Analogamente al controllo PasswordRecovery, è possibile passare da una visualizzazione all'altra tramite lo smart tag del controllo. Inoltre, questi aspetti delle visualizzazioni sono personalizzabili tramite le proprietà di stile assorted o mediante la conversione in un modello.

[![aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Figura 6**: aggiungere un controllo ChangePassword alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image18.png))

Il controllo ChangePassword può aggiornare la password dell'utente attualmente connesso *o* la password di un altro utente specificato. Come illustrato nella figura 6, la visualizzazione predefinita per la modifica della password consente di eseguire solo tre controlli TextBox: uno per la vecchia password e due per la nuova password. Questa interfaccia predefinita viene usata per aggiornare la password dell'utente attualmente connesso.

Per utilizzare il controllo ChangePassword per aggiornare la password di un altro utente, impostare la [proprietà`DisplayUserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) del controllo su true. In questo modo viene aggiunta una quarta casella di testo alla pagina, in cui viene richiesto il nome utente dell'utente la cui password deve essere modificata.

L'impostazione di `DisplayUserName` su true è utile se si desidera consentire a un utente disconnesso di modificare la password senza dover effettuare l'accesso. Personalmente, penso che non ci siano problemi a richiedere a un utente di effettuare l'accesso prima di consentire la modifica della password. Lasciare quindi `DisplayUserName` impostato su false (impostazione predefinita). Per prendere questa decisione, tuttavia, gli utenti anonimi non possono raggiungere questa pagina. Aggiornare le regole di autorizzazione dell'URL del sito in modo da impedire agli utenti anonimi di visitare `ChangePassword.aspx`. Se è necessario aggiornare la memoria nella sintassi della regola di <a id="_msoanchor_4"> </a>autorizzazione dell'URL, fare riferimento all'esercitazione sull' [*autorizzazione basata sull'utente*](../membership/user-based-authorization-cs.md) .

> [!NOTE]
> Potrebbe sembrare che la proprietà `DisplayUserName` sia utile per consentire agli amministratori di modificare le password di altri utenti. Tuttavia, anche quando `DisplayUserName` è impostato su true, è necessario conoscere e immettere la password precedente corretta. Si parlerà di tecniche che consentono agli amministratori di modificare le password degli utenti nel passaggio 3.

Visitare la pagina `ChangePassword.aspx` tramite un browser e modificare la password. Si noti che viene visualizzato un messaggio di errore se si immette una nuova password che non soddisfa i requisiti per la lunghezza della password e i caratteri non alfanumerici specificati nella configurazione delle appartenenze (vedere la figura 7).

[![aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Figura 7**: aggiungere un controllo ChangePassword alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image21.png))

Quando si immette la password precedente corretta e una nuova password valida, la password dell'utente connesso viene modificata e viene visualizzata la visualizzazione Operazione riuscita.

### <a name="sending-a-confirmation-email"></a>Invio di un messaggio di posta elettronica di conferma

Per impostazione predefinita, il controllo ChangePassword non invia un messaggio di posta elettronica all'utente la cui password è stata appena aggiornata. Se si vuole inviare un messaggio di posta elettronica, è sufficiente configurare la proprietà `MailDefinition` del controllo. Verrà ora configurato il controllo ChangePassword in modo che all'utente venga inviato un messaggio di posta elettronica in formato HTML che contiene la nuova password.

Per iniziare, creare un nuovo file nella cartella `EmailTemplates` denominata `ChangePassword.htm`. Aggiungere il markup seguente:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Impostare quindi le proprietà `BodyFileName`, `IsBodyHtml`e `Subject` della proprietà del `MailDefinition` controllo ChangePassword su ~/EmailTemplates/ChangePassword.htm, true e che la password sia stata modificata. rispettivamente.

Dopo aver apportato queste modifiche, rivedere la pagina e modificare di nuovo la password. Questa volta, il controllo ChangePassword Invia un messaggio di posta elettronica in formato HTML personalizzato all'indirizzo di posta elettronica dell'utente nel file (vedere la figura 8).

[![un messaggio di posta elettronica informa l'utente che la password è stata modificata](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Figura 8**: un messaggio di posta elettronica informa l'utente che la password è stata modificata ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Passaggio 3: consentire agli amministratori di modificare le password degli utenti

Una funzionalità comune delle applicazioni che supportano gli account utente è la possibilità per un utente amministratore di modificare le password di altri utenti. Questa funzionalità è a volte necessaria perché il sistema non dispone della possibilità per gli utenti di modificare le proprie password. In tal caso, l'unico modo in cui un utente può recuperare la password dimenticata è l'amministratore ad assegnare loro una nuova password. Con i controlli PasswordRecovery e ChangePassword, tuttavia, gli utenti amministrativi non devono occuparsi di modificare le password degli utenti, in quanto gli utenti sono in grado di eseguire questa operazione.

Ma cosa succede se il client insiste sul fatto che gli utenti amministratori devono essere in grado di modificare le password degli altri utenti? Sfortunatamente, l'aggiunta di questa funzionalità può rivelarsi un po' di lavoro. Per modificare la password di un utente, è necessario fornire la password precedente e la nuova password al metodo `ChangePassword` dell'oggetto `MembershipUser`, ma un amministratore non deve conoscerne la password per modificarlo.

Una soluzione alternativa consiste nel reimpostare la password dell'utente e quindi modificarla con la nuova password usando un codice simile al seguente:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Questo codice inizia recuperando le informazioni relative al *nome utente*, ovvero l'utente la cui password si desidera modificare. Viene quindi richiamato il metodo `ResetPassword`, che assegna a e all'utente una nuova password casuale. Questa password generata in modo casuale viene restituita dal metodo e archiviata nella variabile `resetPwd`. Ora che si conosce la password dell'utente, è possibile modificarla tramite una chiamata a `ChangePassword`.

Il problema è che il codice funziona solo se la configurazione del sistema di appartenenze è impostata in modo tale che `RequiresQuestionAndAnswer` è false. Se `RequiresQuestionAndAnswer` è true, così com'è con l'applicazione, è necessario che al metodo `ResetPassword` venga passata la risposta di sicurezza. in caso contrario, verrà generata un'eccezione.

Se il Framework di appartenenza è configurato in modo da richiedere una domanda e una risposta di sicurezza, ma il client insiste sul fatto che gli amministratori sono in grado di modificare le password degli utenti, sono disponibili tre opzioni:

- Per comunicare al client che si tratta di un'unica cosa che non è possibile eseguire.
- Impostare `RequiresQuestionAndAnswer` su false. In questo modo si ottiene un'applicazione meno sicura. Si supponga che un utente nefasto abbia ottenuto l'accesso alla posta in arrivo di un altro utente. Forse l'utente compromesso ha lasciato la propria scrivania per andare a pranzo e non ha bloccato la workstation o forse ha eseguito l'accesso alla posta elettronica da un terminale pubblico senza disconnettersi. In entrambi i casi, l'utente nefasto può visitare la pagina `RecoverPassword.aspx` e immettere il nome utente dell'utente. Il sistema invia quindi tramite posta elettronica la password ripristinata senza richiedere la risposta di sicurezza.
- Ignorare il livello di astrazione creato dal framework di appartenenza e utilizzare direttamente il database SQL Server. Lo schema di appartenenza include un stored procedure denominato `aspnet_Membership_SetPassword` che imposta la password di un utente e non richiede la risposta di sicurezza o la vecchia password per completare l'attività.

Nessuna di queste opzioni è particolarmente interessante, ma questo è il modo in cui la vita di uno sviluppatore a volte.

Ho implementato il terzo approccio, scrivendo codice che ignora le classi `Membership` e `MembershipUser` e opera direttamente nel database `SecurityTutorials`.

> [!NOTE]
> Lavorando direttamente con il database, l'incapsulamento fornito dal framework di appartenenza viene frantumato. Questa decisione ci lega alla `SqlMembershipProvider`, rendendo il codice meno portabile. Inoltre, questo codice potrebbe non funzionare come previsto nelle future versioni di ASP.NET se lo schema di appartenenza viene modificato. Questo approccio è una soluzione alternativa e, come la maggior parte delle soluzioni alternative, non è un esempio di procedure consigliate.

Il codice presenta alcuni bit non attraenti ed è molto lungo. Quindi, non voglio ingombrare questa esercitazione con un esame approfondito del problema. Se si è interessati a saperne di più, scaricare il codice per questa esercitazione e visitare la pagina `~/Administration/ManageUsers.aspx`. Questa pagina, creata nell' <a id="_msoanchor_5"> </a> [esercitazione precedente](building-an-interface-to-select-one-user-account-from-many-cs.md), elenca ogni utente. Il controllo GridView è stato aggiornato in modo da includere un collegamento alla pagina `UserInformation.aspx`, passando il nome utente dell'utente selezionato tramite la QueryString. Nella pagina `UserInformation.aspx` vengono visualizzate le informazioni sull'utente selezionato e le caselle di testo per la modifica della password (vedere la figura 9).

Dopo aver immesso la nuova password, confermarla nella seconda casella di testo e fare clic sul pulsante Aggiorna utente, viene eseguito un postback e viene richiamato il `aspnet_Membership_SetPassword` stored procedure, che aggiorna la password dell'utente. I lettori interessati a questa funzionalità possono acquisire familiarità con il codice e provare a estendere la funzionalità per includere l'invio di un messaggio di posta elettronica all'utente la cui password è stata modificata.

[![un amministratore può modificare la password di un utente](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Figura 9**: un amministratore può modificare la password di un utente ([fare clic per visualizzare l'immagine con dimensioni complete](recovering-and-changing-passwords-cs/_static/image27.png))

> [!NOTE]
> La pagina `UserInformation.aspx` attualmente funziona solo se il Framework di appartenenza è configurato per archiviare le password in formato non crittografato o con hash. Manca il codice per crittografare la nuova password, anche se si è invitati ad aggiungere questa funzionalità. Il modo in cui si consiglia di aggiungere il codice necessario consiste nell'usare un decompilatore come [Reflector](http://www.aisto.com/roeder/dotnet/) per esaminare il codice sorgente per i metodi nel .NET Framework; per iniziare, esaminare il metodo di `ChangePassword` della classe `SqlMembershipProvider`. Questa è la tecnica utilizzata per scrivere il codice per la creazione di un hash della password.

## <a name="summary"></a>Riepilogo

ASP.NET offre due controlli che consentono agli utenti di gestire la propria password. Il controllo PasswordRecovery è utile per gli utenti che hanno dimenticato la password. A seconda della configurazione del Framework di appartenenza, l'utente invia tramite posta elettronica la password esistente o una nuova password generata in modo casuale. Il controllo ChangePassword consente a un utente di aggiornare la password.

Analogamente ai controlli Login e CreateUserWizard, i controlli PasswordRecovery e ChangePassword eseguono il rendering di un'interfaccia utente avanzata senza dover scrivere un Lick del markup dichiarativo o della riga di codice. Se l'interfaccia utente predefinita non soddisfa le proprie esigenze, è possibile personalizzarla tramite una varietà di proprietà di stile. In alternativa, le interfacce dei controlli possono essere convertite in modelli, per un livello di controllo ancora più preciso. Dietro le quinte questi controlli usano l'API di appartenenza, richiamando i metodi `ResetPassword` e `ChangePassword` dell'oggetto `MembershipUser`.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Guide introduttive al controllo ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Guide introduttive al controllo PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Invio di messaggi di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Domande frequenti `System.Net.Mail`](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione includono Michael Emmings e Suchi bottle. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [Successivo](unlocking-and-approving-user-accounts-cs.md)
