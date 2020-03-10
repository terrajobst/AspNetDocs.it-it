---
uid: signalr/overview/security/introduction-to-security
title: Introduzione alla sicurezza di SignalR | Microsoft Docs
author: bradygaster
description: Descrive i problemi di sicurezza che è necessario prendere in considerazione quando si sviluppa un'applicazione SignalR.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558576"
---
# <a name="introduction-to-signalr-security"></a>Introduzione alla sicurezza di SignalR

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive i problemi di sicurezza che è necessario prendere in considerazione quando si sviluppa un'applicazione SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Concetti sulla sicurezza di SignalR](#concepts)

    - [Autenticazione e autorizzazione](#authentication)
    - [Token di connessione](#connectiontoken)
    - [Riunione di gruppi durante la riconnessione](#rejoingroup)
- [Come SignalR impedisce la falsificazione di richieste tra siti](#csrf)
- [Raccomandazioni sulla sicurezza di SignalR](#recommendations)

    - [Protocollo SSL (Secure Socket Layers)](#ssl)
    - [Non usare i gruppi come meccanismo di sicurezza](#groupsecurity)
    - [Gestione sicura degli input dai client](#input)
    - [Riconciliazione di una modifica dello stato utente con una connessione attiva](#reconcile)
    - [File proxy JavaScript generati automaticamente](#autogen)
    - [Eccezioni](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concetti sulla sicurezza di SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

SignalR non fornisce funzionalità per l'autenticazione degli utenti. Al contrario, si integrano le funzionalità SignalR nella struttura di autenticazione esistente per un'applicazione. Si autenticano gli utenti come si farebbe normalmente nell'applicazione e si lavora con i risultati dell'autenticazione nel codice SignalR. Ad esempio, è possibile autenticare gli utenti con l'autenticazione basata su form ASP.NET e quindi nell'hub, applicare quali utenti o ruoli sono autorizzati a chiamare un metodo. Nell'hub è inoltre possibile passare le informazioni di autenticazione, ad esempio il nome utente o se un utente appartiene a un ruolo, al client.

SignalR fornisce l'attributo di [autorizzazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) per specificare gli utenti che hanno accesso a un hub o a un metodo. Applicare l'attributo autorizzi a un hub o a metodi specifici in un hub. Senza l'attributo Autorizzo, tutti i metodi pubblici nell'hub sono disponibili per un client connesso all'hub. Per altre informazioni sugli hub, vedere [autenticazione e autorizzazione per gli hub SignalR](hub-authorization.md).

Applicare l'attributo `Authorize` a hub, ma non a connessioni permanenti. Per applicare le regole di autorizzazione quando si usa un `PersistentConnection` è necessario eseguire l'override del metodo `AuthorizeRequest`. Per altre informazioni sulle connessioni permanenti, vedere [autenticazione e autorizzazione per le connessioni permanenti SignalR](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token di connessione

SignalR attenua il rischio di esecuzione di comandi dannosi convalidando l'identità del mittente. Per ogni richiesta, il client e il server passano un token di connessione che contiene l'ID connessione e il nome utente per gli utenti autenticati. L'ID connessione identifica in modo univoco ogni client connesso. Il server genera in modo casuale l'ID connessione quando viene creata una nuova connessione e rende permanente tale ID per la durata della connessione. Il meccanismo di autenticazione per l'applicazione Web fornisce il nome utente. SignalR usa la crittografia e una firma digitale per proteggere il token di connessione.

![](introduction-to-security/_static/image2.png)

Per ogni richiesta, il server convalida il contenuto del token per assicurarsi che la richiesta provenga dall'utente specificato. Il nome utente deve corrispondere all'ID connessione. Convalidando sia l'ID connessione che il nome utente, SignalR impedisce a un utente malintenzionato di rappresentare facilmente un altro utente. Se il server non è in grado di convalidare il token di connessione, la richiesta non riesce.

![](introduction-to-security/_static/image4.png)

Poiché l'ID connessione fa parte del processo di verifica, è consigliabile non rivelare l'ID connessione di un utente ad altri utenti o archiviare il valore nel client, ad esempio in un cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Token di connessione rispetto ad altri tipi di token

I token di connessione vengono talvolta contrassegnati dagli strumenti di sicurezza perché sembrano essere token di sessione o token di autenticazione, che costituiscono un rischio se esposti.

Il token di connessione di SignalR non è un token di autenticazione. Viene usato per confermare che l'utente che effettua la richiesta è uguale a quello che ha creato la connessione. Il token di connessione è necessario perché ASP.NET SignalR consente le connessioni per spostarsi tra i server. Il token associa la connessione a un determinato utente, ma non dichiara l'identità dell'utente che effettua la richiesta. Affinché una richiesta SignalR venga autenticata correttamente, deve disporre di un altro token che asserisca l'identità dell'utente, ad esempio un cookie o un bearer token. Tuttavia, il token di connessione non rilascia alcuna attestazione che la richiesta è stata effettuata da tale utente, ma solo che l'ID connessione contenuto all'interno del token è associato a tale utente.

Poiché il token di connessione non fornisce alcuna attestazione di autenticazione, non è considerato un token "Session" o "Authentication". L'acquisizione di un token di connessione di un determinato utente e la riproduzione in una richiesta autenticata come utente diverso (o una richiesta non autenticata) avranno esito negativo perché l'identità utente della richiesta e l'identità archiviata nel token non corrispondono.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Riunione di gruppi durante la riconnessione

Per impostazione predefinita, l'applicazione SignalR riassegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Quando si esegue la riconnessione, il client passa un token di gruppo che include l'ID connessione e i gruppi assegnati. Il token di gruppo è firmato digitalmente e crittografato. Il client mantiene lo stesso ID connessione dopo una riconnessione. Pertanto, l'ID connessione passato dal client riconnesso deve corrispondere all'ID di connessione precedente utilizzato dal client. Questa verifica impedisce a un utente malintenzionato di passare le richieste di join a gruppi non autorizzati durante la riconnessione.

È tuttavia importante notare che il token di gruppo non scade. Se un utente appartiene a un gruppo in passato, ma è stato vietato da tale gruppo, l'utente potrebbe essere in grado di simulare un token di gruppo che include il gruppo vietato. Se è necessario gestire in modo sicuro gli utenti che appartengono a gruppi, è necessario archiviare tali dati nel server, ad esempio in un database. Quindi, aggiungere la logica all'applicazione che verifica nel server se un utente appartiene a un gruppo. Per un esempio di verifica dell'appartenenza a un gruppo, vedere [Working with](../guide-to-the-api/working-with-groups.md)groups.

Il riunione automatica di gruppi si applica solo quando una connessione viene riconnessa dopo un'interrotta temporanea. Se un utente si disconnette allontanandosi dall'applicazione o riavviando l'applicazione, l'applicazione deve gestire come aggiungere tale utente ai gruppi corretti. Per ulteriori informazioni, vedere [Working with groups](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Come SignalR impedisce la falsificazione di richieste tra siti

La richiesta tra siti falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è attualmente connesso. SignalR impedisce CSRF, rendendo estremamente improbabile che un sito dannoso crei una richiesta valida per l'applicazione SignalR.

### <a name="description-of-csrf-attack"></a>Descrizione dell'attacco CSRF

Di seguito è riportato un esempio di un attacco CSRF:

1. Un utente accede a www.example.com, usando l'autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza disconnettersi, l'utente visita un sito Web dannoso. Questo sito dannoso contiene il formato HTML seguente:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Si tratta della parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante Submit (Invia). Il browser include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server example.com con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione consentita a un utente autenticato.

Sebbene questo esempio richieda all'utente di fare clic sul pulsante modulo, è possibile che la pagina dannosa esegua facilmente uno script che invia una richiesta AJAX all'applicazione SignalR. Inoltre, l'uso di SSL non impedisce un attacco CSRF, perché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili per i siti Web che utilizzano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito Web di destinazione. Gli attacchi CSRF, tuttavia, non sono limitati allo sfruttamento dei cookie. Ad esempio, anche l'autenticazione di base e del digest è vulnerabile. Dopo che un utente ha eseguito l'accesso con l'autenticazione di base o del digest, il browser invia automaticamente le credenziali fino alla scadenza della sessione.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigazioni CSRF prese da SignalR

SignalR esegue i passaggi seguenti per impedire a un sito dannoso di creare richieste valide per l'applicazione. SignalR esegue questi passaggi per impostazione predefinita, non è necessario eseguire alcuna operazione nel codice.

- **Disabilitare le richieste tra domini** SignalR Disabilita le richieste tra domini per impedire agli utenti di chiamare un endpoint SignalR da un dominio esterno. SignalR considera la richiesta da un dominio esterno non valida e blocca la richiesta. Si consiglia di rispettare questo comportamento predefinito. in caso contrario, un sito dannoso può indurre gli utenti a inviare comandi al sito. Se è necessario usare le richieste tra domini, vedere [come stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passa il token di connessione nella stringa di query, non nel cookie** SignalR passa il token di connessione come valore della stringa di query, anziché come cookie. Archiviare il token di connessione in un cookie non è sicuro perché il browser può inviare inavvertitamente il token di connessione quando viene rilevato codice dannoso. Inoltre, se si passa il token di connessione nella stringa di query, il token di connessione non viene salvato in modo permanente oltre la connessione corrente. Un utente malintenzionato non può pertanto effettuare una richiesta con le credenziali di autenticazione di un altro utente.
- **Verificare il token di connessione** Come descritto nella sezione [token di connessione](#connectiontoken) , il server sa quale ID di connessione è associato a ciascun utente autenticato. Il server non elabora alcuna richiesta da un ID connessione che non corrisponde al nome utente. È improbabile che un utente malintenzionato possa indovinare una richiesta valida perché l'utente malintenzionato dovrebbe essere a conoscenza del nome utente e dell'ID connessione generato in modo casuale corrente. Tale ID di connessione diventa non valido non appena la connessione viene terminata. Gli utenti anonimi non devono avere accesso a informazioni riservate.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Raccomandazioni sulla sicurezza di SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocollo SSL (Secure Socket Layers)

Il protocollo SSL usa la crittografia per proteggere il trasporto di dati tra un client e un server. Se l'applicazione SignalR trasmette informazioni riservate tra il client e il server, usare SSL per il trasporto. Per ulteriori informazioni sulla configurazione di SSL, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Non usare i gruppi come meccanismo di sicurezza

I gruppi sono un modo pratico per raccogliere gli utenti correlati, ma non sono un meccanismo sicuro per limitare l'accesso alle informazioni riservate. Ciò è particolarmente vero quando gli utenti possono partecipare automaticamente ai gruppi durante una riconnessione. È invece consigliabile aggiungere utenti con privilegi a un ruolo e limitare l'accesso a un metodo hub solo ai membri di tale ruolo. Per un esempio di limitazione dell'accesso in base a un ruolo, vedere [autenticazione e autorizzazione per gli hub SignalR](hub-authorization.md). Per un esempio di verifica dell'accesso utente ai gruppi durante la riconnessione, vedere [Working with groups](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gestione sicura degli input dai client

Per assicurarsi che un utente malintenzionato non invii script ad altri utenti, è necessario codificare tutti gli input da client che devono essere trasmessi ad altri client. È necessario codificare i messaggi nei client riceventi invece che nel server, perché l'applicazione SignalR potrebbe avere molti tipi diversi di client. Di conseguenza, la codifica HTML funziona per un client Web, ma non per altri tipi di client. Ad esempio, un metodo client Web per visualizzare un messaggio di chat gestirebbe in modo sicuro il nome utente e il messaggio chiamando la funzione `html()`.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Riconciliazione di una modifica dello stato utente con una connessione attiva

Se lo stato di autenticazione di un utente viene modificato mentre esiste una connessione attiva, l'utente riceverà un errore che indica che l'identità utente non può essere modificata durante una connessione attiva di SignalR. In tal caso, l'applicazione deve riconnettersi al server per assicurarsi che l'ID connessione e il nome utente siano coordinati. Se, ad esempio, l'applicazione consente all'utente di disconnettersi mentre esiste una connessione attiva, il nome utente per la connessione non corrisponderà più al nome passato per la richiesta successiva. È necessario arrestare la connessione prima che l'utente si disconnetta e quindi riavviarlo.

È tuttavia importante notare che la maggior parte delle applicazioni non dovrà arrestare e avviare manualmente la connessione. Se l'applicazione reindirizza gli utenti a una pagina distinta dopo la disconnessione, ad esempio il comportamento predefinito in un'applicazione Web Form o in un'applicazione MVC oppure aggiorna la pagina corrente dopo la disconnessione, la connessione attiva viene disconnessa automaticamente e non richiedere qualsiasi azione aggiuntiva.

Nell'esempio seguente viene illustrato come arrestare e avviare una connessione quando lo stato dell'utente è cambiato.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

In alternativa, lo stato di autenticazione dell'utente può cambiare se il sito usa la scadenza variabile con l'autenticazione basata su form e non è presente alcuna attività per la validità del cookie di autenticazione. In tal caso, l'utente verrà disconnesso e il nome utente non corrisponderà più al nome utente nel token di connessione. Per risolvere il problema, è possibile aggiungere uno script che richiede periodicamente una risorsa nel server Web per rendere valido il cookie di autenticazione. Nell'esempio seguente viene illustrato come richiedere una risorsa ogni 30 minuti.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>File proxy JavaScript generati automaticamente

Se non si vuole includere tutti gli hub e i metodi nel file proxy JavaScript per ogni utente, è possibile disabilitare la generazione automatica del file. È possibile scegliere questa opzione se si dispone di più hub e metodi, ma non si desidera che tutti gli utenti siano a conoscenza di tutti i metodi. Per disabilitare la generazione automatica, impostare **EnableJavaScriptProxies** su **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Per ulteriori informazioni sui file proxy JavaScript, vedere [il proxy generato e il](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)relativo funzionamento. <a id="exceptions"></a>

### <a name="exceptions"></a>Eccezioni

È consigliabile evitare di passare oggetti eccezione ai client perché gli oggetti possono esporre informazioni riservate ai client. Chiamare invece un metodo sul client che Visualizza il messaggio di errore pertinente.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
