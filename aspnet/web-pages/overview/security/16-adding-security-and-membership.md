---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Aggiunta di sicurezza e appartenenza a un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: In questo capitolo viene illustrato come proteggere il sito Web in modo che alcune pagine siano disponibili solo per gli utenti che accedono. Si vedrà anche come creare pagine...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628387"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Aggiunta di sicurezza e appartenenza a un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come proteggere un sito Web di Pagine Web ASP.NET (Razor) in modo che alcune pagine siano disponibili solo per gli utenti che accedono. Si vedrà anche come creare pagine a cui chiunque può accedere.
> 
> **Cosa si apprenderà:** 
> 
> - Come creare un sito Web con una pagina di registrazione e una pagina di accesso in modo che per alcune pagine è possibile limitare l'accesso solo ai membri.
> - Come creare pagine pubbliche e solo per i membri.
> - Come definire i ruoli, ovvero i gruppi che dispongono di autorizzazioni di sicurezza diverse per il sito e come assegnare utenti a un ruolo.
> - Come utilizzare CAPTCHA per impedire la creazione di account membri da parte di programmi automatizzati (bot).
>   
> 
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
> 
> - Modello di **sito Starter** di WebMatrix.
> - `WebSecurity` helper e la classe `Roles`.
> - Helper `ReCaptcha`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3
> - Libreria helper Web ASP.NET

È possibile configurare il sito Web in modo che gli utenti possano accedervi, in &#8212; modo che il sito supporti l' *appartenenza*. Questa operazione può essere utile per diversi motivi. Ad esempio, il sito potrebbe avere pagine che devono essere disponibili solo per i membri. In alcuni casi, potrebbe essere necessario che gli utenti accedano per inviare commenti e suggerimenti o lasciare un commento.

Anche se il sito Web supporta l'appartenenza, gli utenti non devono necessariamente accedere prima di usare alcune pagine del sito. Gli utenti che non hanno eseguito l'accesso sono noti come *utenti anonimi*.

Un utente può registrarsi sul sito Web e quindi accedere al sito. Il sito Web richiede un nome utente (indirizzo di posta elettronica) e una password per verificare che gli utenti siano quelli che attestano di essere. Questo processo di accesso e conferma dell'identità di un utente è noto come *autenticazione*.

È possibile configurare la sicurezza e l'appartenenza in modi diversi:

- Se si usa WebMatrix, un modo semplice consiste nel creare un nuovo sito basato sul modello di **sito Starter** . Questo modello è già configurato per la sicurezza e l'appartenenza ed è già presente una pagina di registrazione, una pagina di accesso e così via.

    Il sito creato dal modello ha anche la possibilità di consentire agli utenti di accedere usando un sito esterno come Facebook, Google o Twitter.
- Se si desidera aggiungere sicurezza a un sito esistente o se non si desidera utilizzare il modello di **sito iniziale** , è possibile creare una pagina di registrazione personalizzata, una pagina di accesso e così via.

Questo articolo è incentrato sulla prima opzione &mdash; come aggiungere sicurezza tramite il modello di **sito Starter** . Sono inoltre disponibili alcune informazioni di base su come implementare la propria sicurezza e vengono forniti collegamenti a ulteriori informazioni su come eseguire questa operazione. Sono inoltre disponibili informazioni su come abilitare gli account di accesso esterni, descritti in dettaglio in un articolo separato.

## <a name="creating-website-security-using-the-starter-site-template"></a>Creazione della sicurezza del sito Web tramite il modello di sito Starter

In WebMatrix è possibile usare il modello di **sito Starter** per creare un sito Web che contiene quanto segue:

- Database utilizzato per archiviare i nomi utente e le password per i membri.
- Pagina di registrazione in cui è possibile registrare gli utenti anonimi (nuovi).
- Una pagina di accesso e di disconnessione.
- Pagina di ripristino e ripristino della password.

Nella procedura seguente viene descritto come creare il sito e configurarlo.

1. Avviare WebMatrix e nella pagina **avvio rapido** selezionare **sito da modello**.
2. Selezionare il modello di **sito Starter** , quindi fare clic su **OK**. WebMatrix crea un nuovo sito.
3. Nel riquadro sinistro fare clic sul selettore dell'area di lavoro **file** .
4. Nella cartella radice del sito Web aprire il file *\_AppStart. cshtml* , che è un file speciale usato per contenere le impostazioni globali. Contiene alcune istruzioni che sono impostate come commento usando i caratteri `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Queste istruzioni configurano l'helper `WebMail`, che può essere usato per inviare messaggi di posta elettronica. Il sistema di appartenenze può usare la posta elettronica per inviare messaggi di conferma quando gli utenti si registrano o quando vogliono modificare le password. Ad esempio, dopo la registrazione degli utenti, riceveranno un messaggio di posta elettronica che include un collegamento che può fare clic per completare il processo di registrazione.

    L'invio di posta elettronica richiede l'accesso a un server SMTP, come descritto in [aggiunta di un messaggio di posta elettronica a un sito pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202899). Le impostazioni di posta elettronica verranno archiviate in questo file centrale *\_AppStart. cshtml,* in modo che non sia necessario codificarle ripetutamente in ogni pagina in cui è possibile inviare messaggi di posta elettronica. Non è necessario configurare le impostazioni SMTP per configurare un database di registrazione. sono necessarie solo le impostazioni SMTP se si desidera convalidare gli utenti dal proprio alias di posta elettronica e consentire agli utenti di reimpostare una password dimenticata.
5. Rimuovere il commento dalle istruzioni rimuovendo `//` da davanti a ciascuna di esse.

    Se non si vuole configurare la conferma tramite posta elettronica, è possibile ignorare questo passaggio e il passaggio successivo. Se i valori SMTP non sono impostati, il nuovo account sarà immediatamente disponibile senza un messaggio di posta elettronica di conferma.
6. Modificare le seguenti impostazioni relative alla posta elettronica nel codice:

   - Impostare `WebMail.SmtpServer` sul nome del server SMTP a cui si ha accesso.
   - Lasciare `WebMail.EnableSsl` impostato su `true`. Questa impostazione consente di proteggere le credenziali inviate al server SMTP mediante la relativa crittografia.
   - Impostare `WebMail.UserName` sul nome utente per l'account del server SMTP.
   - Impostare `WebMail.Password` sulla password per l'account del server SMTP.
   - Impostare `WebMail.From` per il proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio.

     > [!NOTE] 
     > 
     > **Suggerimento** Per ulteriori informazioni sui valori per queste proprietà, vedere [configurazione delle impostazioni di posta elettronica](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) in [personalizzazione del comportamento a livello di sito per pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Salvare e chiudere *\_AppStart. cshtml*.
8. Eseguire la pagina *default. cshtml* in un browser.

    ![sicurezza-appartenenza-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Se viene visualizzato un errore che indica che una proprietà deve essere un'istanza di `ExtendedMembershipProvider`, è possibile che il sito non sia configurato per utilizzare il sistema di appartenenze Pagine Web ASP.NET (SimpleMembership). Questa situazione può talvolta verificarsi se il server di un provider di hosting è configurato in modo diverso rispetto al server locale. Per risolvere questo problema, aggiungere l'elemento seguente al file *Web. config* del sito:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Aggiungere questo elemento come figlio dell'elemento `<configuration>` e come peer dell'elemento `<system.web>`.
9. Nell'angolo in alto a destra della pagina fare clic sul collegamento **Register (registra** ). Viene visualizzata la pagina *Register. cshtml* .
10. Immettere un nome utente e una password, quindi fare clic su **registra**.

    ![sicurezza-appartenenza-3](16-adding-security-and-membership/_static/image2.png)

    Quando il sito Web è stato creato dal modello di **sito Starter** , viene creato un database denominato *StarterSite. sdf* nella cartella *app\_data* del sito. Durante la registrazione, le informazioni utente vengono aggiunte al database. Se si impostano i valori SMTP, viene inviato un messaggio all'indirizzo di posta elettronica usato per poter completare la registrazione.

    ![sicurezza-appartenenza-4](16-adding-security-and-membership/_static/image3.png)
11. Passare al programma di posta elettronica e trovare il messaggio, che conterrà il codice di conferma e un collegamento ipertestuale al sito.
12. Fare clic sul collegamento ipertestuale per attivare l'account. Il collegamento ipertestuale di conferma apre una pagina di conferma della registrazione.

    ![sicurezza-appartenenza-5](16-adding-security-and-membership/_static/image4.png)
13. Fare clic sul collegamento di **accesso** e quindi accedere con l'account registrato.

      Dopo aver eseguito l'accesso, i collegamenti di **accesso** e **registrazione** vengono sostituiti da un collegamento di **disconnessione** . Il nome dell'account di accesso viene visualizzato come collegamento. Il collegamento consente di passare a una pagina in cui è possibile modificare la password.

      ![sicurezza-appartenenza-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Per impostazione predefinita, le pagine Web ASP.NET inviano le credenziali al server in testo non crittografato, come testo leggibile. Un sito di produzione deve usare il protocollo HTTP protetto (https://, noto anche come *Secure Sockets Layer* o SSL) per crittografare le informazioni riservate scambiate con il server. È possibile richiedere l'invio di messaggi di posta elettronica tramite SSL impostando `WebMail.EnableSsl=true` come nell'esempio precedente. Per ulteriori informazioni su SSL, vedere la pagina relativa alla [protezione delle comunicazioni Web: certificati, SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funzionalità di appartenenza aggiuntiva nel sito

Il sito contiene altre funzionalità che consentono agli utenti di gestire i propri account. Gli utenti possono eseguire le operazioni seguenti:

- Modificare le password. Dopo aver eseguito l'accesso, è possibile fare clic sul nome utente (che è un collegamento). In questo modo si accede a una pagina in cui è possibile creare una nuova password (*account/ChangePassword. cshtml*).
- Recuperare una password dimenticata. Nella pagina di accesso è presente un collegamento (**si dimentica la password?** ) che consente agli utenti di accedere a una pagina (*account/forgotpassword. cshtml*) in cui è possibile immettere un indirizzo di posta elettronica. Il sito invia un messaggio di posta elettronica con un collegamento su cui è possibile fare clic per impostare una nuova password (*account/PasswordReset. cshtml*).

È anche possibile consentire agli utenti di accedere usando un sito esterno, come illustrato in seguito.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Creazione di una pagina di solo membri

Per il momento, chiunque può accedere a qualsiasi pagina nel sito Web. Potrebbe tuttavia essere necessario disporre di pagine che siano disponibili solo per gli utenti che hanno eseguito l'accesso, ovvero ai membri. ASP.NET consente di creare pagine a cui è possibile accedere solo tramite membri connessi. In genere, se gli utenti anonimi tentano di accedere a una pagina di solo membri, vengono reindirizzati alla pagina di accesso.

In questa procedura verrà creata una cartella che conterrà le pagine disponibili solo per gli utenti connessi.

1. Alla radice del sito creare una nuova cartella. Sulla barra multifunzione fare clic sulla freccia sotto **nuova** , quindi scegliere **nuova cartella**.
2. Assegnare un nome ai nuovi *membri*della cartella.
3. Nella cartella *membri* creare una nuova pagina e denominarla *MembersInformation. cshtml*.
4. Sostituire il contenuto esistente con il codice e il markup seguenti:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Questo codice testa la proprietà `IsAuthenticated` dell'oggetto `WebSecurity`, che restituisce `true` se l'utente ha eseguito l'accesso. Se l'utente non è connesso, il codice chiama `Response.Redirect` per inviare l'utente alla pagina *login. cshtml* nella cartella *account* .

    L'URL del reindirizzamento include un `returnUrl` valore della stringa di query che utilizza `Request.Url.LocalPath` per impostare il percorso della pagina corrente. Se si imposta il valore `returnUrl` nella stringa di query come questa (e se l'URL restituito è un percorso locale), la pagina di accesso restituirà gli utenti a questa pagina dopo l'accesso.

    Il codice imposta anche *\_pagina SiteLayout. cshtml* come pagina di layout. Per ulteriori informazioni sulle pagine di layout, vedere la pagina relativa [alla creazione di un layout coerente nei siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).
5. Eseguire il sito. Se si è ancora connessi, fare clic sul pulsante **Disconnetti** nella parte superiore della pagina.
6. Nel browser, richiedere la pagina */Members/MembersInformation*. Ad esempio, l'URL potrebbe essere simile al seguente:

    `http://localhost:38366/Members/MembersInformation`

    Il numero di porta (38366) sarà probabilmente diverso nell'URL.

    Si viene reindirizzati alla pagina *login. cshtml* , perché non è stato effettuato l'accesso.
7. Accedere con l'account creato in precedenza. Si viene reindirizzati di nuovo alla pagina *MembersInformation* . Poiché è stato eseguito l'accesso, questa volta viene visualizzato il contenuto della pagina.

Per proteggere l'accesso a più pagine, è possibile eseguire questa operazione:

- Aggiungere il controllo di sicurezza a ogni pagina.
- Creare una pagina *\_PageStart. cshtml* nella cartella in cui si conservano le pagine protette e aggiungere il controllo di sicurezza. La pagina *\_PageStart. cshtml* funge da pagina globale per tutte le pagine della cartella. Questa tecnica è illustrata in modo più dettagliato nella [personalizzazione del comportamento a livello di sito per pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Creazione della sicurezza per gruppi di utenti (ruoli)

Se il sito ha un numero elevato di membri, non è efficiente controllare l'autorizzazione per ogni singolo utente prima di visualizzare una pagina. È invece possibile creare gruppi o *ruoli*a cui appartengono i singoli membri. È quindi possibile controllare le autorizzazioni in base al ruolo. In questa sezione verrà creato un ruolo amministratore &quot;&quot; e quindi verrà creata una pagina accessibile agli utenti che fanno parte di tale ruolo.

Il sistema di appartenenze ASP.NET è configurato per supportare i ruoli. Tuttavia, a differenza della registrazione e dell'accesso all'appartenenza, il modello di **sito Starter** non contiene pagine che consentono di gestire i ruoli. (La gestione dei ruoli è un'attività amministrativa anziché un'attività utente). È tuttavia possibile aggiungere gruppi direttamente nel database delle appartenenze in WebMatrix.

1. In WebMatrix fare clic sul selettore dell'area di lavoro **database** .
2. Nel riquadro sinistro aprire il nodo *StarterSite. sdf* , aprire il nodo **tabelle** , quindi fare doppio clic sulla tabella *pagine Web\_ruoli* .

    ![sicurezza-appartenenza-7](16-adding-security-and-membership/_static/image6.png)
3. Aggiungere un ruolo denominato &quot;&quot;amministratore. Il campo *RoleId* viene compilato automaticamente. (Si tratta della chiave primaria ed è stato impostato come campo di identificazione, come illustrato in [Introduzione all'uso di un database nei siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)).
4. Prendere nota del valore del campo *RoleId* . Se si tratta del primo ruolo che si sta definendo, sarà 1.

    ![sicurezza-appartenenza-8](16-adding-security-and-membership/_static/image7.png)
5. Chiudere le *pagine web\_tabella Roles* .
6. Aprire la tabella *USERPROFILE* .
7. Prendere nota del valore *userid* di uno o più utenti nella tabella e quindi chiudere la tabella.
8. Aprire le *pagine web\_tabella UserInRoles* e immettere un *ID utente* e un valore *roleId* nella tabella. Ad esempio, per inserire l'utente 2 nel ruolo &quot;amministratore&quot;, immettere i valori seguenti:

    ![sicurezza-appartenenza-9](16-adding-security-and-membership/_static/image8.png)
9. Chiudere le *pagine web\_tabella UsersInRoles* .

    Ora che è stato definito un ruolo, è possibile configurare una pagina accessibile agli utenti che si trovano in tale ruolo.
10. Nella cartella radice del sito Web creare una nuova pagina denominata *AdminError. cshtml* e sostituire il contenuto esistente con il codice seguente. Si tratta della pagina a cui gli utenti vengono reindirizzati se non sono autorizzati ad accedere a una pagina.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Nella cartella radice del sito Web creare una nuova pagina denominata *AdminOnly. cshtml* e sostituire il codice esistente con il codice seguente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Il metodo `Roles.IsUserInRole` restituisce `true` se l'utente corrente è un membro del ruolo specificato (in questo caso, il ruolo "admin").
12. Eseguire *default. cshtml* in un browser, ma non eseguire l'accesso. (Se si è già connessi, disconnettersi).
13. Nella barra degli indirizzi del browser aggiungere *AdminOnly* nell'URL. (In altre parole, richiedere il file *AdminOnly. cshtml* ). Si viene reindirizzati alla pagina *AdminError. cshtml* , perché non si è attualmente connessi come utente nel ruolo di amministratore di &quot;&quot;.
14. Tornare a *default. cshtml* e accedere con l'utente aggiunto al ruolo di amministratore di &quot;&quot;.
15. Passare alla pagina *AdminOnly. cshtml* . Questa volta viene visualizzata la pagina.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedire ai programmi automatizzati di aggiungersi al sito Web

La pagina di accesso non arresterà i programmi automatizzati (talvolta detti *robot Web* o *bot*) dalla registrazione con il sito Web. Questa procedura descrive come abilitare un test di reCAPTCHA per la pagina di registrazione.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Al termine della registrazione, si otterranno una chiave pubblica e una chiave privata.
2. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
3. Nella cartella *account* aprire il file denominato *Register. cshtml*.
4. Nel codice nella parte superiore della pagina trovare le righe seguenti e rimuovere il commento rimuovendo i caratteri di commento `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Sostituire `PRIVATE_KEY` con la propria chiave privata reCaptcha.
6. Nel markup della pagina rimuovere il `@*` e `*@` i caratteri di commento da circa le righe seguenti nel markup della pagina:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Sostituire `PUBLIC_KEY` con la chiave.
8. Se non è già stato rimosso, rimuovere l'elemento `<div>` che contiene il testo che inizia con "per abilitare la verifica CAPTCHA...". Rimuovere l'intero elemento `<div>` e il relativo contenuto.

9. Eseguire *default. cshtml* in un browser. Se si è connessi al sito, fare clic sul collegamento di **disconnessione** .
10. Fare clic sul collegamento **Register** e testare la registrazione usando il test CAPTCHA.

     ![sicurezza-appartenenza-10](16-adding-security-and-membership/_static/image9.png)

Per ulteriori informazioni sull'helper `ReCaptcha`, vedere l'argomento relativo all' [uso di un CATPCHA per impedire che i programmi automatici (bot) utilizzino il sito Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Consentire agli utenti di accedere con un sito esterno

Il modello di **sito Starter** include codice e markup che consente agli utenti di accedere con Facebook, Windows Live, Twitter, Google o Yahoo. Per impostazione predefinita, questa funzionalità non è abilitata. La procedura generale per l'uso di consentire agli utenti di accedere usando questi provider esterni è la seguente:

- Decidere quali siti esterni si desidera supportare.
- Se necessario, passare a tale sito e configurare un'app di accesso. Ad esempio, è necessario eseguire questa operazione per consentire gli account di accesso di Facebook.
- Configurare il provider nel sito. Nella maggior parte dei casi, è sufficiente rimuovere il commento dal codice nel file *\_AppStart. cshtml* .
- Aggiungere markup alla pagina di registrazione che consente agli utenti di collegarsi al sito esterno per l'accesso. In genere è possibile copiare il markup necessario e modificare leggermente il testo.

È possibile trovare istruzioni dettagliate nell'argomento [Abilitazione dell'accesso da siti esterni in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969).

Dopo che un utente ha eseguito l'accesso da un altro sito, l'utente torna al sito e lo *associa* al sito. In effetti, viene creata una voce di appartenenza nel sito per l'accesso esterno dell'utente. In questo modo è possibile usare le normali funzionalità di appartenenza, ad esempio i ruoli, con l'account di accesso esterno.

## <a name="adding-security-to-an-existing-website"></a>Aggiunta della sicurezza a un sito Web esistente

La procedura descritta in precedenza in questo articolo si basa sull'uso del modello di **sito Starter** come base per la sicurezza del sito Web. Se non si è pratici per iniziare dal modello di **sito Starter** o copiare le pagine pertinenti da un sito basato su tale modello, è possibile implementare lo stesso tipo di sicurezza nel proprio sito codificando manualmente. Si creano gli stessi tipi di pagine, ovvero registrazione, accesso e così via, e quindi si utilizzano helper e classi per configurare l'appartenenza.

Il processo di base è descritto nel post del Blog [il modo più semplice per implementare la sicurezza di ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La maggior parte del lavoro viene eseguita utilizzando i metodi e le proprietà seguenti dell'helper `WebSecurity`:

- [WebSecurty. UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity. CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Questi metodi consentono di determinare se un utente è già registrato e di registrarli.
- [WebSecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Questa proprietà consente di determinare se l'utente corrente ha eseguito l'accesso. Questa operazione è utile per reindirizzare gli utenti a una pagina di accesso, se non hanno già effettuato l'accesso.
- [WebSecurity. login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity. logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Questi metodi registrano un utente all'interno o all'esterno.
- [WebSecurity. CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Questa proprietà è utile per visualizzare il nome di accesso dell'utente corrente (se l'utente è connesso).
- [WebSecurity. ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Questo metodo è utile se si configura la conferma della posta elettronica per la registrazione. I dettagli sono descritti nel post di Blog [relativo all'uso della funzionalità di conferma per pagine Web ASP.NET sicurezza](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).

Per gestire i ruoli, è possibile usare le classi [roles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) e [Membership](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) , come descritto nel post di Blog.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Protezione delle comunicazioni Web: certificati, SSL e https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Il modo più semplice per implementare la sicurezza Razor di ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) e [usare la funzionalità di conferma per pagine Web ASP.NET sicurezza](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Questi sono post di Blog che descrivono come implementare le funzionalità di appartenenza a ASP.NET senza usare il modello di **sito Starter** .
- [Abilitazione dell'accesso da siti esterni in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- Informazioni di [riferimento sulle API della classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- Informazioni di [riferimento sulle API della classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- Informazioni di [riferimento sulle API della classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
