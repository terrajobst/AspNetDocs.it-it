---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorizzazione basata sull'utente (C#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come limitare l'accesso alle pagine e come limitare la funzionalità a livello di pagina tramite diverse tecniche.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 059dbf42956268884dcfdade696491ac39e32da9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614668"
---
# <a name="user-based-authorization-c"></a>Autorizzazione basata sull'utente (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> In questa esercitazione verrà illustrato come limitare l'accesso alle pagine e come limitare la funzionalità a livello di pagina tramite diverse tecniche.

## <a name="introduction"></a>Introduzione

La maggior parte delle applicazioni Web che offrono account utente consente di limitare l'accesso di determinati utenti a determinate pagine all'interno del sito. Nella maggior parte dei siti messageboard online, ad esempio tutti gli utenti anonimi e autenticati, sono in grado di visualizzare i post di messageboard, ma solo gli utenti autenticati possono visitare la pagina Web per creare un nuovo post. Potrebbero essere presenti pagine amministrative accessibili solo a un determinato utente (o a un particolare gruppo di utenti). Inoltre, le funzionalità a livello di pagina possono variare in base agli utenti. Quando si visualizza un elenco di post, agli utenti autenticati viene mostrata un'interfaccia per la classificazione di ogni post, mentre questa interfaccia non è disponibile per i visitatori anonimi.

ASP.NET semplifica la definizione delle regole di autorizzazione basate sugli utenti. Con un semplice markup in `Web.config`, è possibile bloccare pagine Web specifiche o intere directory in modo che siano accessibili solo a un subset di utenti specificato. La funzionalità a livello di pagina può essere attivata o disattivata in base all'utente attualmente connesso tramite un metodo programmatico e dichiarativo.

In questa esercitazione verrà illustrato come limitare l'accesso alle pagine e come limitare la funzionalità a livello di pagina tramite diverse tecniche. Iniziamo!

## <a name="a-look-at-the-url-authorization-workflow"></a>Esaminare il flusso di lavoro di autorizzazione URL

Come illustrato nell'esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-cs.md) , quando il runtime ASP.NET elabora una richiesta di una risorsa ASP.NET, la richiesta genera un certo numero di eventi durante il ciclo di vita. I *moduli HTTP* sono classi gestite il cui codice viene eseguito in risposta a un evento specifico nel ciclo di vita della richiesta. ASP.NET viene fornito con una serie di moduli HTTP che eseguono attività essenziali dietro le quinte.

Uno di questi moduli HTTP è [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Come illustrato nelle esercitazioni precedenti, la funzione principale del `FormsAuthenticationModule` consiste nel determinare l'identità della richiesta corrente. Questa operazione viene eseguita controllando il ticket di autenticazione basata su form, che si trova in un cookie o incorporato all'interno dell'URL. Questa identificazione si verifica durante l' [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Un altro modulo HTTP importante è il [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), che viene generato in risposta all' [evento di`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (che si verifica dopo l'evento di `AuthenticateRequest`). Il `UrlAuthorizationModule` esamina il markup di configurazione in `Web.config` per determinare se l'identità corrente dispone dell'autorità per visitare la pagina specificata. Questo processo è denominato *autorizzazione URL*.

Si esaminerà la sintassi per le regole di autorizzazione dell'URL nel passaggio 1, ma prima di tutto si osserverà il risultato della `UrlAuthorizationModule` a seconda che la richiesta sia autorizzata o meno. Se il `UrlAuthorizationModule` determina che la richiesta è autorizzata, non esegue alcuna operazione e la richiesta continua con il ciclo di vita. Tuttavia, se la richiesta non è autorizzata, il `UrlAuthorizationModule` interrompe il ciclo di vita e indica all'oggetto `Response` di restituire uno stato *non* [autorizzato HTTP 401](http://www.checkupdown.com/status/E401.html) . Quando si usa l'autenticazione basata su form, questo stato HTTP 401 non viene mai restituito al client perché se il `FormsAuthenticationModule` rileva uno stato HTTP 401, lo stato viene modificato in un [reindirizzamento http 302](http://www.checkupdown.com/status/E302.html) alla pagina di accesso.

Nella figura 1 viene illustrato il flusso di lavoro della pipeline ASP.NET, il `FormsAuthenticationModule`e il `UrlAuthorizationModule` quando arriva una richiesta non autorizzata. In particolare, nella figura 1 viene illustrata una richiesta da parte di un visitatore anonimo per `ProtectedPage.aspx`, ovvero una pagina che nega l'accesso agli utenti anonimi. Poiché il visitatore è anonimo, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato non autorizzato HTTP 401. Il `FormsAuthenticationModule` converte quindi lo stato 401 in una pagina di accesso Reindirizzamento a 302. Dopo che l'utente è stato autenticato tramite la pagina di accesso, viene reindirizzato a `ProtectedPage.aspx`. Questa volta il `FormsAuthenticationModule` identifica l'utente in base al ticket di autenticazione. Ora che il visitatore è autenticato, il `UrlAuthorizationModule` consente l'accesso alla pagina.

[![il flusso di lavoro di autenticazione basata su form e autorizzazione URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: flusso di lavoro di autenticazione basata su form e autorizzazione URL ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image3.png))

Nella figura 1 viene illustrata l'interazione che si verifica quando un visitatore anonimo tenta di accedere a una risorsa che non è disponibile per gli utenti anonimi. In tal caso, il visitatore anonimo viene reindirizzato alla pagina di accesso con la pagina che ha tentato di visitare specificata nella QueryString. Una volta che l'utente ha effettuato l'accesso, verrà automaticamente reindirizzato alla risorsa che stava tentando inizialmente di visualizzare.

Quando la richiesta non autorizzata viene effettuata da un utente anonimo, questo flusso di lavoro è semplice ed è facile per il visitatore capire cosa si è verificato e perché. Tenere tuttavia presente che il `FormsAuthenticationModule` reindirizza *eventuali* utenti non autorizzati alla pagina di accesso, anche se la richiesta viene effettuata da un utente autenticato. Questo può comportare un'esperienza utente confusa se un utente autenticato tenta di visitare una pagina per la quale non dispone di autorità.

Si supponga che il sito Web abbia le regole di autorizzazione URL configurate in modo che la pagina ASP.NET `OnlyTito.aspx` sia accessibilmente solo a Tito. A questo punto, si supponga che Sam visiti il sito, acceda e tenti di visitare `OnlyTito.aspx`. Il `UrlAuthorizationModule` arresterà il ciclo di vita della richiesta e restituirà uno stato non autorizzato HTTP 401, che verrà rilevato dal `FormsAuthenticationModule` e quindi reindirizza Sam alla pagina di accesso. Poiché Sam ha già eseguito l'accesso, tuttavia, può chiedersi perché è stato inviato nuovamente alla pagina di accesso. Potrebbe essere il motivo per cui le credenziali di accesso sono state perse in qualche modo o che ha immesso credenziali non valide. Se Sam riimmette le proprie credenziali nella pagina di accesso, si sarà connessi (nuovamente) e verrà reindirizzato a `OnlyTito.aspx`. Il `UrlAuthorizationModule` rileverà che Sam non può visitare questa pagina e verrà restituita alla pagina di accesso.

Nella figura 2 viene illustrato questo flusso di lavoro confuso.

[![il flusso di lavoro predefinito può causare un ciclo confusione](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: il flusso di lavoro predefinito può causare un ciclo confusionario ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image6.png))

Il flusso di lavoro illustrato nella figura 2 può Befuddle rapidamente anche il visitatore più esperto di computer. Si esamineranno i modi per evitare questo ciclo di confusione nel passaggio 2.

> [!NOTE]
> ASP.NET utilizza due meccanismi per determinare se l'utente corrente può accedere a una particolare pagina Web: autorizzazione URL e autorizzazione file. L'autorizzazione file viene implementata dal [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), che determina l'autorità consultando gli ACL dei file richiesti. L'autorizzazione per i file viene usata più di frequente con l'autenticazione di Windows poiché gli ACL sono autorizzazioni valide per gli account di Windows. Quando si usa l'autenticazione basata su form, tutte le richieste a livello di sistema operativo e di file system vengono eseguite dallo stesso account di Windows, indipendentemente dall'utente che visita il sito. Poiché questa serie di esercitazioni è incentrata sull'autenticazione basata su form, non verrà discussa l'autorizzazione per i file.

### <a name="the-scope-of-url-authorization"></a>Ambito dell'autorizzazione URL

Il `UrlAuthorizationModule` è codice gestito che fa parte del runtime di ASP.NET. Prima della versione 7 del server Web di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) , esisteva una barriera distinta tra la pipeline HTTP di IIS e la pipeline del runtime di ASP.NET. In breve, in IIS 6 e versioni precedenti, ASP. La `UrlAuthorizationModule` di rete viene eseguita solo quando una richiesta viene delegata da IIS al runtime di ASP.NET. Per impostazione predefinita, IIS elabora il contenuto statico, ad esempio pagine HTML e CSS, JavaScript e file di immagine, e invia le richieste al runtime ASP.NET solo quando viene richiesta una pagina con estensione `.aspx`, `.asmx`o `.ashx`.

IIS 7, tuttavia, consente le pipeline IIS e ASP.NET integrate. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per richiamare il `UrlAuthorizationModule` per *tutte* le richieste, vale a dire che è possibile definire le regole di autorizzazione dell'URL per i file di qualsiasi tipo. Inoltre, IIS 7 include il proprio motore di autorizzazione URL. Per ulteriori informazioni sull'integrazione di ASP.NET e sulla funzionalità di autorizzazione URL nativa di IIS 7, vedere [informazioni sull'autorizzazione dell'URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Per un'analisi più approfondita dell'integrazione di ASP.NET e IIS 7, è prevedibile una copia del libro di Khosravi di, *Professional IIS 7 e ASP.NET Integrated Programming* (ISBN: 978-0470152539).

In breve, nelle versioni precedenti a IIS 7, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime di ASP.NET. Con IIS 7 è tuttavia possibile utilizzare la funzionalità di autorizzazione URL nativa di IIS o per integrare ASP. `UrlAuthorizationModule` NET nella pipeline HTTP di IIS, estendendo così questa funzionalità a tutte le richieste.

> [!NOTE]
> Esistono alcune differenze significative ma importanti per la modalità di ASP. La funzionalità di autorizzazione URL di NET `UrlAuthorizationModule` e IIS 7 elabora le regole di autorizzazione. In questa esercitazione non vengono esaminate le funzionalità di autorizzazione URL di IIS 7 o le differenze nella modalità di analisi delle regole di autorizzazione rispetto al `UrlAuthorizationModule`. Per ulteriori informazioni su questi argomenti, vedere la documentazione di IIS 7 su MSDN o all'indirizzo [www.IIS.NET](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Passaggio 1: definizione delle regole di autorizzazione URL in`Web.config`

Il `UrlAuthorizationModule` determina se concedere o negare l'accesso a una risorsa richiesta per una determinata identità in base alle regole di autorizzazione URL definite nella configurazione dell'applicazione. Le regole di autorizzazione vengono digitate nell' [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) sotto forma di `<allow>` e `<deny>` elementi figlio. Ogni `<allow>` e `<deny>` elemento figlio può specificare:

- Un utente specifico
- Elenco delimitato da virgole di utenti
- Tutti gli utenti anonimi, identificati da un punto interrogativo (?)
- Tutti gli utenti, identificati da un asterisco (\*)

Il markup seguente illustra come usare le regole di autorizzazione dell'URL per consentire agli utenti Tito e Scott e di negare tutti gli altri:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

L'elemento `<allow>` definisce quali utenti sono consentiti, Tito e Scott, mentre l'elemento `<deny>` indica che *tutti* gli utenti sono stati negati.

> [!NOTE]
> Gli elementi `<allow>` e `<deny>` possono inoltre specificare le regole di autorizzazione per i ruoli. Si esaminerà l'autorizzazione basata sui ruoli in un'esercitazione futura.

L'impostazione seguente concede l'accesso a utenti diversi da Sam (inclusi i visitatori anonimi):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Per consentire solo gli utenti autenticati, usare la configurazione seguente, che nega l'accesso a tutti gli utenti anonimi:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Le regole di autorizzazione vengono definite all'interno dell'elemento `<system.web>` in `Web.config` e si applicano a tutte le risorse ASP.NET nell'applicazione Web. Spesso un'applicazione dispone di regole di autorizzazione diverse per le diverse sezioni. Ad esempio, in un sito di eCommerce, tutti i visitatori possono sfogliare i prodotti, vedere le recensioni dei prodotti, eseguire ricerche nel catalogo e così via. Tuttavia, solo gli utenti autenticati possono raggiungere il checkout o le pagine per gestire la cronologia delle spedizioni. Potrebbero inoltre essere presenti parti del sito accessibili solo agli utenti selezionati, ad esempio gli amministratori del sito.

ASP.NET semplifica la definizione di regole di autorizzazione diverse per file e cartelle diversi nel sito. Le regole di autorizzazione specificate nel file di `Web.config` della cartella radice si applicano a tutte le risorse ASP.NET nel sito. Tuttavia, è possibile eseguire l'override di queste impostazioni di autorizzazione predefinite per una cartella specifica aggiungendo un `Web.config` con una sezione di `<authorization>`.

Aggiornare il sito Web in modo che solo gli utenti autenticati possano visitare le pagine ASP.NET nella cartella `Membership`. A tale scopo, è necessario aggiungere un file di `Web.config` alla cartella `Membership` e impostare le impostazioni di autorizzazione per negare gli utenti anonimi. Fare clic con il pulsante destro del mouse sulla cartella `Membership` nel Esplora soluzioni, scegliere il menu Aggiungi nuovo elemento dal menu di scelta rapida e aggiungere un nuovo file di configurazione Web denominato `Web.config`.

[![aggiungere un file Web. config alla cartella di appartenenza](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: aggiungere un File di `Web.config` alla cartella `Membership` ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image9.png))

A questo punto il progetto deve contenere due file di `Web.config`: uno nella directory radice e uno nella cartella `Membership`.

[![l'applicazione dovrebbe ora contenere due file Web. config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: l'applicazione dovrebbe ora contenere due file di `Web.config` ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image12.png))

Aggiornare il file di configurazione nella cartella `Membership` in modo da impedire l'accesso agli utenti anonimi.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Questo è tutto.

Per testare questa modifica, visitare la Home page in un browser e assicurarsi di essere disconnessi. Poiché il comportamento predefinito di un'applicazione ASP.NET è consentire tutti i visitatori e poiché non sono state apportate modifiche di autorizzazione al file di `Web.config` della directory radice, è possibile visitare i file nella directory radice come visitatore anonimo.

Fare clic sul collegamento creazione di account utente disponibile nella colonna a sinistra. Verrà visualizzata la `~/Membership/CreatingUserAccounts.aspx`. Poiché il file `Web.config` nella cartella `Membership` definisce le regole di autorizzazione per impedire l'accesso anonimo, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Il `FormsAuthenticationModule` lo modifica in uno stato di reindirizzamento 302, inviando la pagina di accesso. Si noti che la pagina a cui si sta tentando di accedere (`CreatingUserAccounts.aspx`) viene passata alla pagina di accesso tramite il `ReturnUrl` parametro QueryString.

[![poiché le regole di autorizzazione URL non consentono l'accesso anonimo, viene reindirizzato alla pagina di accesso](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: poiché le regole di autorizzazione dell'URL proibiscono l'accesso anonimo, si viene reindirizzati alla pagina di accesso ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image15.png))

Dopo aver eseguito l'accesso, si verrà reindirizzati alla pagina `CreatingUserAccounts.aspx`. Questa volta il `UrlAuthorizationModule` consente l'accesso alla pagina perché non è più anonimo.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Applicazione delle regole di autorizzazione URL a una posizione specifica

Le impostazioni di autorizzazione definite nella sezione `<system.web>` di `Web.config` si applicano a tutte le risorse di ASP.NET nella directory e nelle relative sottodirectory (fino a quando non viene eseguito l'override di un altro file di `Web.config`). In alcuni casi, tuttavia, potrebbe essere necessario che tutte le risorse di ASP.NET in una determinata directory dispongano di una particolare configurazione di autorizzazione, ad eccezione di una o due pagine specifiche. Questa operazione può essere eseguita aggiungendo un elemento `<location>` in `Web.config`, puntando al file le cui regole di autorizzazione sono diverse e definendo le relative regole di autorizzazione univoche.

Per illustrare l'uso dell'elemento `<location>` per eseguire l'override delle impostazioni di configurazione per una risorsa specifica, è possibile personalizzare le impostazioni di autorizzazione in modo da consentire solo a Tito di visitare `CreatingUserAccounts.aspx`. A tale scopo, aggiungere un elemento `<location>` al file `Web.config` della cartella `Membership` e aggiornarne il markup in modo che abbia un aspetto simile al seguente:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

L'elemento `<authorization>` in `<system.web>` definisce le regole di autorizzazione URL predefinite per le risorse ASP.NET nella cartella `Membership` e nelle relative sottocartelle. L'elemento `<location>` consente di eseguire l'override di queste regole per una determinata risorsa. Nel markup precedente l'elemento `<location>` fa riferimento alla pagina `CreatingUserAccounts.aspx` e specifica le regole di autorizzazione, ad esempio per consentire Tito, ma negare tutti gli altri.

Per testare questa modifica dell'autorizzazione, iniziare visitando il sito Web come utente anonimo. Se si tenta di visitare qualsiasi pagina nella cartella `Membership`, ad esempio `UserBasedAuthorization.aspx`, il `UrlAuthorizationModule` negherà la richiesta e si verrà reindirizzati alla pagina di accesso. Dopo aver eseguito l'accesso come, ad esempio, Scott, è possibile visitare qualsiasi pagina nella cartella `Membership` *ad eccezione* di `CreatingUserAccounts.aspx`. Il tentativo di visitare `CreatingUserAccounts.aspx` connessi come utente, ma Tito comporterà un tentativo di accesso non autorizzato, reindirizzando di nuovo la pagina di accesso.

> [!NOTE]
> L'elemento `<location>` deve essere visualizzato all'esterno dell'elemento `<system.web>` della configurazione. È necessario usare un elemento `<location>` separato per ogni risorsa di cui si vuole eseguire l'override delle impostazioni di autorizzazione.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Esaminare il modo in cui il`UrlAuthorizationModule`usa le regole di autorizzazione per concedere o negare l'accesso

Il `UrlAuthorizationModule` determina se autorizzare una determinata identità per un particolare URL analizzando le regole di autorizzazione dell'URL una alla volta, a partire dal primo e funzionante. Non appena viene rilevata una corrispondenza, all'utente viene concesso o negato l'accesso, a seconda se la corrispondenza è stata trovata in un elemento `<allow>` o `<deny>`. <strong>Se non viene trovata alcuna corrispondenza, all'utente viene concesso l'accesso.</strong> Di conseguenza, se si vuole limitare l'accesso, è fondamentale usare un elemento `<deny>` come ultimo elemento nella configurazione dell'autorizzazione URL. <strong>Se si omette un</strong> elemento<strong>`<deny>`</strong> <strong>, viene concesso l'accesso a tutti gli utenti.</strong>

Per comprendere meglio il processo usato dalla `UrlAuthorizationModule` per determinare l'autorità, prendere in considerazione le regole di autorizzazione dell'URL di esempio esaminate in precedenza in questo passaggio. La prima regola è un elemento `<allow>` che consente l'accesso a Tito e Scott. La seconda regola è un elemento `<deny>` che nega l'accesso a tutti gli utenti. Se un utente anonimo visita, il `UrlAuthorizationModule` inizia chiedendo, è anonimo o Scott o Tito? La risposta, ovviamente, è No, quindi procede alla seconda regola. È anonimo nel set di tutti? Poiché la risposta è sì, la regola `<deny>` viene applicata e il visitatore viene reindirizzato alla pagina di accesso. Analogamente, se Jisun viene visitato, il `UrlAuthorizationModule` inizia chiedendo, è Jisun Scott o Tito? Dal momento che non lo è, il `UrlAuthorizationModule` procede alla seconda domanda, Jisun nel set di tutti? A questo proposito, viene negato l'accesso. Infine, se Tito visita, la prima domanda rappresentata dal `UrlAuthorizationModule` è una risposta affermativa, quindi Tito viene concesso l'accesso.

Poiché il `UrlAuthorizationModule` elabora le regole di autorizzazione dall'alto verso il basso, arrestandosi in corrispondenza di qualsiasi corrispondenza, è importante che le regole più specifiche vengano prima di quelle meno specifiche. Ovvero, per definire le regole di autorizzazione che impediscono l'Jisun e gli utenti anonimi, ma consentono tutti gli altri utenti autenticati, si inizierà con la regola più specifica, ovvero quella che ha un effetto su Jisun e quindi si procede con le regole meno specifiche, che consentono tutti gli altri utenti autenticati, ma che negano tutti gli utenti anonimi. Le seguenti regole di autorizzazione URL implementano questi criteri negando prima Jisun e quindi negando qualsiasi utente anonimo. A qualsiasi utente autenticato diverso da Jisun verrà concesso l'accesso perché nessuna di queste `<deny>` istruzioni corrisponderà.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Passaggio 2: correzione del flusso di lavoro per utenti autenticati non autorizzati

Come illustrato in precedenza in questa esercitazione, nella sezione relativa al flusso di lavoro di autorizzazione URL, ogni volta che una richiesta non autorizzata traspare, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Questo stato 401 viene modificato dal `FormsAuthenticationModule` in uno stato di reindirizzamento 302 che invia l'utente alla pagina di accesso. Questo flusso di lavoro viene eseguito su qualsiasi richiesta non autorizzata, anche se l'utente è autenticato.

È probabile che la restituzione di un utente autenticato alla pagina di accesso consentirà di confonderle perché sono già state registrate nel sistema. Con un po' di lavoro è possibile migliorare questo flusso di lavoro reindirizzando gli utenti autenticati che effettuano richieste non autorizzate a una pagina che spiega che hanno tentato di accedere a una pagina con restrizioni.

Per iniziare, creare una nuova pagina ASP.NET nella cartella radice dell'applicazione Web denominata `UnauthorizedAccess.aspx`; non dimenticare di associare questa pagina alla pagina master `Site.master`. Dopo aver creato questa pagina, rimuovere il controllo contenuto che fa riferimento al `LoginContent` ContentPlaceHolder in modo che venga visualizzato il contenuto predefinito della pagina master. Aggiungere quindi un messaggio che spieghi la situazione, ovvero che l'utente ha tentato di accedere a una risorsa protetta. Dopo l'aggiunta di un messaggio di questo tipo, il markup dichiarativo della pagina `UnauthorizedAccess.aspx` dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

A questo punto è necessario modificare il flusso di lavoro in modo che se una richiesta non autorizzata viene eseguita da un utente autenticato, questi vengono inviati alla pagina `UnauthorizedAccess.aspx` invece che alla pagina di accesso. La logica che reindirizza le richieste non autorizzate alla pagina di accesso viene nascosta all'interno di un metodo privato della classe `FormsAuthenticationModule`, pertanto non è possibile personalizzare questo comportamento. Tuttavia, è possibile aggiungere la propria logica alla pagina di accesso per reindirizzare l'utente a `UnauthorizedAccess.aspx`, se necessario.

Quando il `FormsAuthenticationModule` reindirizza un visitatore non autorizzato alla pagina di accesso, accoda l'URL richiesto, non autorizzato, alla QueryString con il nome `ReturnUrl`. Se, ad esempio, un utente non autorizzato tenta di visitare `OnlyTito.aspx`, il `FormsAuthenticationModule` li reindirizza al `Login.aspx?ReturnUrl=OnlyTito.aspx`. Se quindi la pagina di accesso viene raggiunta da un utente autenticato con una stringa QueryString che include il parametro `ReturnUrl`, è possibile che l'utente non autenticato abbia tentato di visitare una pagina che non è autorizzato a visualizzare. In tal caso, si desidera reindirizzare l'utente a `UnauthorizedAccess.aspx`.

A tale scopo, aggiungere il codice seguente al gestore dell'evento `Page_Load` della pagina di accesso:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Il codice riportato sopra reindirizza gli utenti autenticati e non autorizzati alla pagina `UnauthorizedAccess.aspx`. Per visualizzare questa logica in azione, visitare il sito come visitatore anonimo e fare clic sul collegamento creazione di account utente nella colonna a sinistra. Verrà visualizzata la pagina `~/Membership/CreatingUserAccounts.aspx`, che nel passaggio 1 è stato configurato in modo da consentire l'accesso solo a Tito. Poiché gli utenti anonimi non sono consentiti, il `FormsAuthenticationModule` reindirizza nuovamente alla pagina di accesso.

A questo punto è anonimo, quindi `Request.IsAuthenticated` restituisce `false` e non viene reindirizzato al `UnauthorizedAccess.aspx`. Viene invece visualizzata la pagina di accesso. Accedere come utente diverso da Tito, ad esempio Bruce. Dopo aver immesso le credenziali appropriate, la pagina di accesso reindirizza a `~/Membership/CreatingUserAccounts.aspx`. Tuttavia, poiché questa pagina è accessibile solo a Tito, l'utente non è autorizzato a visualizzarlo e viene restituito tempestivamente alla pagina di accesso. Questa volta, tuttavia, `Request.IsAuthenticated` restituisce `true` (e il `ReturnUrl` parametro QueryString esiste), quindi viene reindirizzato alla pagina `UnauthorizedAccess.aspx`.

[![autenticati, gli utenti non autorizzati vengono reindirizzati a UnauthorizedAccess. aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: gli utenti autenticati e non autorizzati vengono reindirizzati a `UnauthorizedAccess.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image18.png))

Questo flusso di lavoro personalizzato presenta un'esperienza utente più sensata e semplice mediante cortocircuito del ciclo illustrato nella figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Passaggio 3: limitazione della funzionalità in base all'utente attualmente connesso

L'autorizzazione URL consente di specificare facilmente le regole di autorizzazione grossolane. Come illustrato nel passaggio 1, con l'autorizzazione URL è possibile indicare in modo conciso quali identità sono consentite e quali vengono negate alla visualizzazione di una particolare pagina o di tutte le pagine in una cartella. In alcuni scenari, tuttavia, potrebbe essere necessario consentire a tutti gli utenti di visitare una pagina, ma limitare le funzionalità della pagina in base all'utente che lo visita.

Si consideri il caso di un sito Web di eCommerce che consente ai visitatori autenticati di esaminare i prodotti. Quando un utente anonimo visita la pagina di un prodotto, Visualizza solo le informazioni sul prodotto e non ha la possibilità di uscire da una recensione. Tuttavia, un utente autenticato che visita la stessa pagina vedrà l'interfaccia di revisione. Se l'utente autenticato non ha ancora esaminato questo prodotto, l'interfaccia consentirebbe di inviare una verifica; in caso contrario, verrebbe visualizzata la revisione inviata in precedenza. Per eseguire ulteriormente questo scenario, la pagina del prodotto potrebbe visualizzare informazioni aggiuntive e offrire funzionalità estese per gli utenti che lavorano per l'azienda di eCommerce. La pagina del prodotto, ad esempio, potrebbe elencare l'inventario in magazzino e includere opzioni per modificare il prezzo e la descrizione del prodotto quando visitati da un dipendente.

Le regole di autorizzazione per granularità fine possono essere implementate in modo dichiarativo o a livello di codice o tramite una combinazione dei due. Nella sezione successiva verrà illustrato come implementare l'autorizzazione con granularità fine tramite il controllo LoginView. In seguito, si esamineranno le tecniche programmatiche. Prima di poter esaminare l'applicazione di regole di autorizzazione granulari, è necessario innanzitutto creare una pagina la cui funzionalità dipende dall'utente che la visita.

Viene ora creata una pagina in cui sono elencati i file in una particolare directory all'interno di un controllo GridView. Oltre a elencare il nome, le dimensioni e altre informazioni di ogni file, GridView includerà due colonne di LinkButton: una per la visualizzazione denominata e una con titolo Delete. Se si fa clic sull'LinkButton di visualizzazione, verrà visualizzato il contenuto del file selezionato; Se si fa clic su Elimina LinkButton, il file verrà eliminato. Si creerà inizialmente questa pagina in modo che la relativa funzionalità di visualizzazione ed eliminazione sia disponibile per tutti gli utenti. Nella sezione Utilizzo del controllo LoginView e limitazione delle funzionalità a livello di codice si vedrà come abilitare o disabilitare queste funzionalità in base all'utente che visita la pagina.

> [!NOTE]
> La pagina ASP.NET da compilare usa un controllo GridView per visualizzare un elenco di file. Poiché questa serie di esercitazioni è incentrata sull'autenticazione basata su form, sull'autorizzazione, sugli account utente e sui ruoli, non desidero dedicare troppo tempo alla discussione dei lavori interni del controllo GridView. Sebbene in questa esercitazione siano disponibili istruzioni dettagliate per la configurazione di questa pagina, non vengono esaminati i dettagli del motivo per cui sono state apportate determinate scelte o l'effetto sulle proprietà specifiche dell'output sottoposto a rendering. Per un esame approfondito del controllo GridView, vedere la serie di esercitazioni sull' *[uso dei dati in ASP.NET 2,0](../../data-access/index.md)* .

Per iniziare, aprire il file `UserBasedAuthorization.aspx` nella cartella `Membership` e aggiungere un controllo GridView alla pagina denominata `FilesGrid`. Dallo smart tag di GridView fare clic sul collegamento Modifica colonne per aprire la finestra di dialogo campi. Da qui, deselezionare la casella di controllo genera automaticamente campi nell'angolo in basso a sinistra. Aggiungere quindi un pulsante Select, un pulsante Delete e due BoundField dall'angolo superiore sinistro (i pulsanti SELECT ed Delete si trovano nel tipo CommandField). Impostare la proprietà `SelectText` del pulsante Seleziona su Visualizza e le proprietà `HeaderText` e `DataField` del primo BoundField su nome. Impostare la proprietà `HeaderText` del secondo BoundField su size in byte, la relativa proprietà `DataField` su length, la relativa proprietà `DataFormatString` su {0:N0} e la relativa proprietà `HtmlEncode` su false.

Dopo aver configurato le colonne di GridView, fare clic su OK per chiudere la finestra di dialogo campi. Dalla Finestra Proprietà impostare la proprietà `DataKeyNames` di GridView su `FullName`. A questo punto, il markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Con il markup di GridView creato, è possibile scrivere il codice che recupererà i file in una determinata directory e li associerà a GridView. Aggiungere il codice seguente al gestore dell'evento `Page_Load` della pagina:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Il codice precedente usa la [classe`DirectoryInfo`](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) per ottenere un elenco dei file nella cartella radice dell'applicazione. Il [metodo`GetFiles()`](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) restituisce tutti i file nella directory come una matrice di [oggetti`FileInfo`](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), che viene quindi associata a GridView. L'oggetto `FileInfo` dispone di un'ampia gamma di proprietà, ad esempio `Name`, `Length`e `IsReadOnly`, tra le altre. Come si può notare dal markup dichiarativo, GridView visualizza solo le proprietà `Name` e `Length`.

> [!NOTE]
> Le classi `DirectoryInfo` e `FileInfo` si trovano nello [spazio dei nomi`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Pertanto, è necessario anteporre questi nomi di classe con i nomi degli spazi dei nomi o fare in modo che lo spazio dei nomi sia importato nel file di classe (tramite `using System.IO`).

Visita la pagina tramite un browser. Verrà visualizzato l'elenco dei file che risiedono nella directory radice dell'applicazione. Se si fa clic su uno degli LinkButton di visualizzazione o di eliminazione, si verificherà un postback, ma non verrà eseguita alcuna azione perché è ancora necessario creare i gestori eventi necessari.

[![GridView elenca i file nella directory radice dell'applicazione Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: GridView elenca i file nella directory radice dell'applicazione Web ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image21.png))

È necessario un mezzo per visualizzare il contenuto del file selezionato. Tornare a Visual Studio e aggiungere una casella di testo denominata `FileContents` sopra GridView. Impostare la relativa proprietà `TextMode` su `MultiLine` e le proprietà `Columns` e `Rows` rispettivamente su 95% e 10.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per l' [evento`SelectedIndexChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) di GridView e aggiungere il codice seguente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Questo codice usa la proprietà `SelectedValue` di GridView per determinare il nome file completo del file selezionato. Internamente, per ottenere la `SelectedValue`viene fatto riferimento alla raccolta di `DataKeys`, quindi è fondamentale impostare la proprietà `DataKeyNames` di GridView su Name, come descritto in precedenza in questo passaggio. La [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) viene utilizzata per leggere il contenuto del file selezionato in una stringa, che viene quindi assegnata alla proprietà `Text` della `FileContents` casella di testo, visualizzando quindi il contenuto del file selezionato nella pagina.

[![il contenuto del file selezionato viene visualizzato nella casella di testo](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: il contenuto del file selezionato viene visualizzato nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image24.png))

> [!NOTE]
> Se si Visualizza il contenuto di un file che contiene markup HTML, quindi si tenta di visualizzare o eliminare un file, si riceverà un errore di `HttpRequestValidationException`. Questo problema si verifica perché durante il postback il contenuto della casella di testo viene restituito al server Web. Per impostazione predefinita, ASP.NET genera un errore di `HttpRequestValidationException` ogni volta che viene rilevato contenuto di postback potenzialmente pericoloso, ad esempio il markup HTML. Per impedire che si verifichi questo errore, disattivare la convalida delle richieste per la pagina aggiungendo `ValidateRequest="false"` alla direttiva `@Page`. Per ulteriori informazioni sui vantaggi derivanti dalla convalida delle richieste e sulle precauzioni da intraprendere per la relativa disabilitazione, vedere la pagina relativa alla [convalida delle richieste che impedisce gli attacchi di script](https://asp.net/learn/whitepapers/request-validation/).

Infine, aggiungere un gestore eventi con il codice seguente per l' [evento`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)di GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Il codice visualizza semplicemente il nome completo del file da eliminare nella casella di testo `FileContents` *senza* eliminare effettivamente il file.

[![facendo clic sul pulsante Elimina non viene effettivamente eliminato il file](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: facendo clic sul pulsante Elimina il file non viene effettivamente eliminato ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image27.png))

Nel passaggio 1 sono state configurate le regole di autorizzazione URL per impedire agli utenti anonimi di visualizzare le pagine nella cartella `Membership`. Per una migliore esposizione dell'autenticazione con granularità fine, consentire agli utenti anonimi di visitare la pagina `UserBasedAuthorization.aspx`, ma con funzionalità limitate. Per aprire questa pagina per accedere a tutti gli utenti, aggiungere il seguente elemento `<location>` al file `Web.config` nella cartella `Membership`:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Dopo aver aggiunto questo elemento `<location>`, testare le nuove regole di autorizzazione URL disconnettendosi dal sito. Un utente anonimo dovrebbe essere autorizzato a visitare la pagina `UserBasedAuthorization.aspx`.

Attualmente, qualsiasi utente autenticato o anonimo può visitare la pagina `UserBasedAuthorization.aspx` e visualizzare o eliminare i file. Si può fare in modo che solo gli utenti autenticati possano visualizzare il contenuto di un file e solo Tito possa eliminare un file. Le regole di autorizzazione per granularità fine possono essere applicate in modo dichiarativo, a livello di codice o tramite una combinazione di entrambi i metodi. Si userà l'approccio dichiarativo per limitare gli utenti che possono visualizzare il contenuto di un file; per limitare gli utenti che possono eliminare un file, verrà usato l'approccio programmatico.

### <a name="using-the-loginview-control"></a>Uso del controllo LoginView

Come illustrato nelle esercitazioni precedenti, il controllo LoginView è utile per visualizzare interfacce diverse per utenti autenticati e anonimi e offre un modo semplice per nascondere funzionalità non accessibili agli utenti anonimi. Poiché gli utenti anonimi non possono visualizzare o eliminare i file, è sufficiente visualizzare la casella di testo `FileContents` quando la pagina viene visitata da un utente autenticato. A tale scopo, aggiungere un controllo LoginView alla pagina, denominarlo `LoginViewForFileContentsTextBox`e spostare il markup dichiarativo `FileContents` casella di testo nel `LoggedInTemplate`del controllo LoginView.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

I controlli Web nei modelli di LoginView non sono più direttamente accessibili dalla classe code-behind. Ad esempio, i gestori eventi di `SelectedIndexChanged` e `RowDeleting` di GridView `FilesGrid` fanno attualmente riferimento al controllo TextBox `FileContents` con codice simile al seguente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Tuttavia, questo codice non è più valido. Spostando la casella di testo `FileContents` in `LoggedInTemplate` non è possibile accedere direttamente alla casella di testo. Al contrario, è necessario usare il metodo `FindControl("controlId")` per fare riferimento a un controllo a livello di codice. Aggiornare i gestori eventi `FilesGrid` per fare riferimento alla casella di testo in modo analogo al seguente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Dopo aver spostato la casella di testo nel `LoggedInTemplate` di LoginView e aver aggiornato il codice della pagina in modo che faccia riferimento alla casella di testo usando il modello di `FindControl("controlId")`, visitare la pagina come utente anonimo. Come illustrato nella figura 10, la casella di testo `FileContents` non viene visualizzata. Tuttavia, la visualizzazione LinkButton è ancora visualizzata.

[![il controllo LoginView esegue il rendering solo della casella di testo fileContents per gli utenti autenticati](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: il controllo LoginView esegue il rendering solo della casella di testo `FileContents` per gli utenti autenticati ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image30.png))

Un modo per nascondere il pulsante Visualizza per gli utenti anonimi consiste nel convertire il campo GridView in un TemplateField. Verrà generato un modello che contiene il markup dichiarativo per il LinkButton della vista. È quindi possibile aggiungere un controllo LoginView a TemplateField e inserire il LinkButton all'interno del `LoggedInTemplate`di LoginView, nascondendo quindi il pulsante View da visitatori anonimi. A tale scopo, fare clic sul collegamento Modifica colonne dallo smart tag di GridView per avviare la finestra di dialogo campi. Selezionare quindi il pulsante Seleziona dall'elenco nell'angolo in basso a sinistra e quindi fare clic sul collegamento Converti questo campo in un TemplateField. In questo modo, il markup dichiarativo del campo viene modificato da:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

A questo punto, è possibile aggiungere un oggetto LoginView a TemplateField. Nel markup seguente viene visualizzato l'oggetto LinkButton di visualizzazione solo per gli utenti autenticati.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Come illustrato nella figura 11, il risultato finale non è analogo a quanto la colonna della vista viene ancora visualizzata anche se gli oggetti LinkButton della vista nella colonna sono nascosti. Nella sezione successiva verrà illustrato come nascondere l'intera colonna GridView (e non solo LinkButton).

[![il controllo LoginView nasconde gli LinkButton di visualizzazione per i visitatori anonimi](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: il controllo LoginView nasconde gli LinkButton di visualizzazione per i visitatori anonimi ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitazione delle funzionalità a livello di codice

In alcune circostanze, le tecniche dichiarative non sono sufficienti per limitare la funzionalità a una pagina. È ad esempio possibile che la disponibilità di determinate funzionalità della pagina dipenda da criteri che esulano dal fatto che l'utente che visita la pagina sia anonimo o autenticato. In questi casi, i vari elementi dell'interfaccia utente possono essere visualizzati o nascosti tramite mezzi programmatici.

Per limitare la funzionalità a livello di codice, è necessario eseguire due attività:

1. Determinare se l'utente che visita la pagina può accedere alla funzionalità e
2. Modificare a livello di codice l'interfaccia utente a seconda che l'utente abbia accesso alla funzionalità in questione.

Per illustrare l'applicazione di queste due attività, è possibile consentire solo a Tito di eliminare i file dal controllo GridView. La prima attività, quindi, consiste nel determinare se è Tito visitare la pagina. Una volta determinato, è necessario nascondere o mostrare la colonna Delete di GridView. Le colonne di GridView sono accessibili tramite la relativa proprietà `Columns`; viene eseguito il rendering di una colonna solo se la relativa proprietà `Visible` è impostata su `true` (impostazione predefinita).

Aggiungere il codice seguente al gestore eventi `Page_Load` prima di associare i dati a GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Come illustrato nell'esercitazione [*Panoramica dell'autenticazione basata su form*](../introduction/an-overview-of-forms-authentication-cs.md) , `User.Identity.Name` restituisce il nome dell'identità. Corrisponde al nome utente immesso nel controllo di accesso. Se è Tito visitare la pagina, la proprietà `Visible` della seconda colonna di GridView è impostata su `true`; in caso contrario, è impostato su `false`. Il risultato finale è che quando un utente diverso da Tito visita la pagina, ovvero un altro utente autenticato o un utente anonimo, non viene eseguito il rendering della colonna Delete (vedere la figura 12). Tuttavia, quando Tito visita la pagina, è presente la colonna Delete (vedere la figura 13).

[![non viene eseguito il rendering della colonna DELETE se visitata da un utente diverso da Tito (ad esempio Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: non viene eseguito il rendering della colonna DELETE se visitata da un utente diverso da Tito (ad esempio Bruce) ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image36.png))

[![viene eseguito il rendering della colonna Delete per Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: viene eseguito il rendering della colonna Delete per Tito ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Passaggio 4: applicazione di regole di autorizzazione a classi e metodi

Nel passaggio 3 non è stato consentito agli utenti anonimi di visualizzare il contenuto di un file e non è stato consentito a tutti gli utenti, ma a Tito di eliminare i file. Questa operazione è stata eseguita nascondendo gli elementi dell'interfaccia utente associati per i visitatori non autorizzati tramite tecniche dichiarative e a livello di codice. Per questo semplice esempio, nascondere correttamente gli elementi dell'interfaccia utente è stato semplice, ma cosa accade per i siti più complessi in cui possono essere disponibili diversi modi per eseguire la stessa funzionalità? Quando si limita la funzionalità a utenti non autorizzati, cosa accade se si dimentica di nascondere o disabilitare tutti gli elementi dell'interfaccia utente applicabili?

Un modo semplice per evitare che un utente non autorizzato possa accedere a una particolare parte della funzionalità consiste nel decorare tale classe o metodo con l' [attributo`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il Runtime .NET usa una classe o esegue uno dei relativi metodi, verifica che il contesto di sicurezza corrente disponga delle autorizzazioni per usare la classe o eseguire il metodo. L'attributo `PrincipalPermission` fornisce un meccanismo tramite il quale è possibile definire queste regole.

Viene ora illustrato l'utilizzo dell'attributo `PrincipalPermission` sui `SelectedIndexChanged` e `RowDeleting` gestori di eventi di GridView per impedire l'esecuzione da parte di utenti anonimi e utenti diversi da Tito, rispettivamente. È sufficiente aggiungere l'attributo appropriato in cima a ogni definizione di funzione:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

L'attributo per il gestore dell'evento `SelectedIndexChanged` impone che solo gli utenti autenticati possano eseguire il gestore eventi, dove l'attributo nel gestore dell'evento `RowDeleting` limita l'esecuzione a Tito.

Se, in qualche modo, un utente diverso da Tito tenta di eseguire il gestore dell'evento `RowDeleting` o un utente non autenticato tenta di eseguire il gestore dell'evento `SelectedIndexChanged`, il runtime di .NET genererà un `SecurityException`.

[![se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figura 14**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'`SecurityException` ([fare clic per visualizzare l'immagine con dimensioni complete](user-based-authorization-cs/_static/image42.png))

> [!NOTE]
> Per consentire a più contesti di sicurezza di accedere a una classe o a un metodo, decorare la classe o il metodo con un attributo `PrincipalPermission` per ogni contesto di sicurezza. Per consentire a Tito e Bruce di eseguire il gestore dell'evento `RowDeleting`, aggiungere *due* attributi `PrincipalPermission`:

[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Oltre alle pagine ASP.NET, molte applicazioni hanno anche un'architettura che include diversi livelli, ad esempio la logica di business e i livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e offrono classi e metodi per l'esecuzione di funzionalità relative alla logica di business e ai dati. L'attributo `PrincipalPermission` è utile per applicare le regole di autorizzazione a tali livelli.

Per altre informazioni sull'uso dell'attributo `PrincipalPermission` per definire le regole di autorizzazione per le classi e i metodi, vedere la voce del Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/) [aggiunta di regole di autorizzazione ai livelli business e dati usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato esaminato come applicare le regole di autorizzazione basate sugli utenti. È stata avviata un'occhiata a ASP. Framework di autorizzazione dell'URL di NET. Per ogni richiesta, il `UrlAuthorizationModule` del motore ASP.NET controlla le regole di autorizzazione dell'URL definite nella configurazione dell'applicazione per determinare se l'identità è autorizzata ad accedere alla risorsa richiesta. In breve, l'autorizzazione URL consente di specificare facilmente le regole di autorizzazione per una pagina specifica o per tutte le pagine in una directory specifica.

Il Framework di autorizzazione URL applica le regole di autorizzazione in base alla pagina. Con l'autorizzazione URL, l'identità richiedente è autorizzata ad accedere a una determinata risorsa. Molti scenari, tuttavia, chiamano per le regole di autorizzazione granularità più fine. Anziché definire gli utenti autorizzati ad accedere a una pagina, potrebbe essere necessario consentire a tutti di accedere a una pagina, ma per visualizzare dati diversi o offrire funzionalità diverse a seconda dell'utente che visita la pagina. Per evitare che utenti non autorizzati accedano alla funzionalità non consentita, l'autorizzazione a livello di pagina comporta in genere l'occultamento di elementi specifici dell'interfaccia utente. Inoltre, è possibile utilizzare gli attributi per limitare l'accesso alle classi e l'esecuzione dei relativi metodi per determinati utenti.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Aggiunta di regole di autorizzazione ai livelli business e dati mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Modifiche tra IIS6 e IIS7 Security](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurazione di file e sottodirectory specifici](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitazione della funzionalità di modifica dei dati in base all'utente](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Guide introduttive del controllo LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Informazioni sull'autorizzazione dell'URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [Documentazione tecnica di `UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Uso dei dati in ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Successivo](storing-additional-user-information-cs.md)
