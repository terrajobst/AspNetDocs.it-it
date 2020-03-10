---
uid: signalr/overview/older-versions/introduction-to-security
title: Introduzione alla sicurezza di SignalR (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: Descrive i problemi di sicurezza che è necessario prendere in considerazione quando si sviluppa un'applicazione SignalR.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 34172c0a2a15a7ab0d782704d5831ce236f5c989
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536729"
---
# <a name="introduction-to-signalr-security-signalr-1x"></a>Introduzione alla sicurezza di SignalR (SignalR 1.x)

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive i problemi di sicurezza che è necessario prendere in considerazione quando si sviluppa un'applicazione SignalR.

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

SignalR è progettato per essere integrato nella struttura di autenticazione esistente per un'applicazione. Non fornisce funzionalità per l'autenticazione degli utenti. Al contrario, si autenticano gli utenti come si farebbe normalmente nell'applicazione e quindi si lavora con i risultati dell'autenticazione nel codice SignalR. Ad esempio, è possibile autenticare gli utenti con l'autenticazione basata su form ASP.NET e quindi nell'hub, applicare quali utenti o ruoli sono autorizzati a chiamare un metodo. Nell'hub è inoltre possibile passare le informazioni di autenticazione, ad esempio il nome utente o se un utente appartiene a un ruolo, al client.

SignalR fornisce l'attributo di [autorizzazione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) per specificare gli utenti che hanno accesso a un hub o a un metodo. Applicare l'attributo autorizzi a un hub o a metodi specifici in un hub. Senza l'attributo Autorizzo, tutti i metodi pubblici nell'hub sono disponibili per un client connesso all'hub. Per altre informazioni sugli hub, vedere [autenticazione e autorizzazione per gli hub SignalR](../security/hub-authorization.md).

L'attributo `Authorize` viene usato solo con gli hub. Per applicare le regole di autorizzazione quando si usa un `PersistentConnection` è necessario eseguire l'override del metodo `AuthorizeRequest`. Per altre informazioni sulle connessioni permanenti, vedere [autenticazione e autorizzazione per le connessioni permanenti SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token di connessione

SignalR attenua il rischio di esecuzione di comandi dannosi convalidando l'identità del mittente. Un token di connessione, contenente l'ID connessione e il nome utente per gli utenti autenticati, viene passato tra il client e il server per ogni richiesta. L'ID connessione è un identificatore univoco che viene generato in modo casuale dal server quando viene creata una nuova connessione e viene mantenuto per la durata della connessione. Il nome utente viene fornito dal meccanismo di autenticazione per l'applicazione Web. Il token di connessione è protetto con crittografia e firma digitale.

![](introduction-to-security/_static/image2.png)

Per ogni richiesta, il server convalida il contenuto del token per assicurarsi che la richiesta provenga dall'utente specificato. Il nome utente deve corrispondere all'ID connessione. Convalidando sia l'ID connessione che il nome utente, SignalR impedisce a un utente malintenzionato di rappresentare facilmente un altro utente. Se il server non è in grado di convalidare il token di connessione, la richiesta non riesce.

![](introduction-to-security/_static/image4.png)

Poiché l'ID connessione fa parte del processo di verifica, è consigliabile non rivelare l'ID connessione di un utente ad altri utenti o archiviare il valore nel client, ad esempio in un cookie.

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

1. Un utente accede `www.example.com`usando l'autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza disconnettersi, l'utente visita un sito Web dannoso. Questo sito dannoso contiene il formato HTML seguente: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Si tratta della parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante Submit (Invia). Il browser include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server example.com con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione consentita a un utente autenticato.

Sebbene questo esempio richieda all'utente di fare clic sul pulsante modulo, è possibile che la pagina dannosa esegua facilmente uno script che invia una richiesta AJAX all'applicazione SignalR. Inoltre, l'uso di SSL non impedisce un attacco CSRF, perché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili per i siti Web che utilizzano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito Web di destinazione. Gli attacchi CSRF, tuttavia, non sono limitati allo sfruttamento dei cookie. Ad esempio, anche l'autenticazione di base e del digest è vulnerabile. Dopo che un utente ha eseguito l'accesso con l'autenticazione di base o del digest, il browser invia automaticamente le credenziali fino alla scadenza della sessione.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigazioni CSRF prese da SignalR

SignalR esegue i passaggi seguenti per impedire a un sito dannoso di creare richieste valide per l'applicazione SignalR. Questi passaggi vengono eseguiti per impostazione predefinita e non richiedono alcuna azione nel codice.

- **Disabilitare le richieste tra domini**  
 Per impostazione predefinita, le richieste tra domini sono disabilitate in un'applicazione SignalR per impedire agli utenti di chiamare un endpoint SignalR da un dominio esterno. Tutte le richieste provenienti da un dominio esterno vengono automaticamente considerate non valide e bloccate. È consigliabile rispettare questo comportamento predefinito. in caso contrario, un sito dannoso può indurre gli utenti a inviare comandi al sito. Se è necessario usare le richieste tra domini, vedere [come stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passa il token di connessione nella stringa di query, non nel cookie**  
 SignalR passa il token di connessione come valore della stringa di query, anziché come cookie. Non archiviando il token di connessione come cookie, il token di connessione non viene inavvertitamente inviato dal browser quando viene rilevato codice dannoso. Inoltre, il token di connessione non viene salvato in maniera permanente oltre la connessione corrente. Un utente malintenzionato non può pertanto effettuare una richiesta con le credenziali di autenticazione di un altro utente.
- **Verificare il token di connessione**  
 Come descritto nella sezione [token di connessione](#connectiontoken) , il server sa quale ID di connessione è associato a ciascun utente autenticato. Il server non elabora alcuna richiesta da un ID connessione che non corrisponde al nome utente. È improbabile che un utente malintenzionato possa indovinare una richiesta valida perché l'utente malintenzionato dovrebbe essere a conoscenza del nome utente e dell'ID connessione generato in modo casuale corrente. Tale ID di connessione diventa non valido non appena la connessione viene terminata. Gli utenti anonimi non devono avere accesso a informazioni riservate.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Raccomandazioni sulla sicurezza di SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocollo SSL (Secure Socket Layers)

Il protocollo SSL usa la crittografia per proteggere il trasporto di dati tra un client e un server. Se l'applicazione SignalR trasmette informazioni riservate tra il client e il server, usare SSL per il trasporto. Per ulteriori informazioni sulla configurazione di SSL, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Non usare i gruppi come meccanismo di sicurezza

I gruppi sono un modo pratico per raccogliere gli utenti correlati, ma non sono un meccanismo sicuro per limitare l'accesso alle informazioni riservate. Ciò è particolarmente vero quando gli utenti possono partecipare automaticamente ai gruppi durante una riconnessione. È invece consigliabile aggiungere utenti con privilegi a un ruolo e limitare l'accesso a un metodo hub solo ai membri di tale ruolo. Per un esempio di limitazione dell'accesso in base a un ruolo, vedere [autenticazione e autorizzazione per gli hub SignalR](../security/hub-authorization.md). Per un esempio di verifica dell'accesso utente ai gruppi durante la riconnessione, vedere [Working with groups](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gestione sicura degli input dai client

Tutti gli input dei client destinati alla trasmissione ad altri client devono essere codificati per garantire che un utente malintenzionato non invii script ad altri utenti. È consigliabile codificare i messaggi nei client riceventi invece che nel server, perché l'applicazione SignalR potrebbe avere molti tipi diversi di client. Di conseguenza, la codifica HTML funziona per un client Web, ma non per altri tipi di client. Ad esempio, un metodo client Web per visualizzare un messaggio di chat gestirebbe in modo sicuro il nome utente e il messaggio chiamando la funzione `html()`.

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
