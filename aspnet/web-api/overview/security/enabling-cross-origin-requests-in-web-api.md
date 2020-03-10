---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitazione di richieste tra le origini in API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come supportare la condivisione di risorse tra le origini (CORS) in API Web ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555706"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Abilitare le richieste tra le origini in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

> La sicurezza del browser impedisce a una pagina Web di creare richieste AJAX per un altro dominio. Questa restrizione è detta *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito. Tuttavia, in alcuni casi potrebbe essere necessario lasciare che altri siti chiamino l'API Web.
>
> La [condivisione di risorse tra le origini](http://www.w3.org/TR/cors/) (CORS) è uno standard W3C che consente a un server di ridurre i criteri di origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre. CORS è più sicuro e flessibile rispetto alle tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP). Questa esercitazione illustra come abilitare CORS nell'applicazione API Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Software usato nell'esercitazione
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2,2

## <a name="introduction"></a>Introduzione

Questa esercitazione illustra il supporto di CORS in API Web ASP.NET. Si inizierà creando due progetti ASP.NET, uno denominato "WebService", che ospita un controller API Web e l'altro denominato "WebClient", che chiama WebService. Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient a WebService è una richiesta tra origini.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Che cos'è la stessa origine?

Due URL hanno la stessa origine se hanno schemi, host e porte identici. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Questi due URL hanno la stessa origine:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Questi URL hanno origini diverse rispetto alle due precedenti:

- `http://example.net`-dominio diverso
- `http://example.com:9000/foo.html`-porta diversa
- `https://example.com/foo.html`-schema diverso
- `http://www.example.com/foo.html`-sottodominio diverso

> [!NOTE]
> Internet Explorer non considera la porta durante il confronto delle origini.

## <a name="create-the-webservice-project"></a>Creare il progetto WebService

> [!NOTE]
> Questa sezione presuppone che si sappia già come creare progetti API Web. In caso contrario, vedere [Introduzione con API Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Avviare Visual Studio e creare un nuovo progetto di **applicazione Web ASP.NET (.NET Framework)** .
2. Nella finestra di dialogo **nuova applicazione Web ASP.NET** selezionare il modello di progetto **vuoto** . In **Aggiungi cartelle e riferimenti principali per**selezionare la casella di controllo **API Web** .

   ![Finestra di dialogo nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Aggiungere un controller API Web denominato `TestController` con il codice seguente:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. È possibile eseguire l'applicazione localmente o distribuirla in Azure. Per le schermate di questa esercitazione, l'app viene distribuita in app Azure app Web del servizio. Per verificare che l'API Web funzioni, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è stata distribuita l'applicazione. Dovrebbe essere visualizzato il testo della risposta &quot;GET: test&quot;Message.

   ![Web browser visualizzazione del messaggio di prova](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Creare il progetto WebClient

1. Creare un altro progetto di **applicazione Web ASP.NET (.NET Framework)** e selezionare il modello di progetto **MVC** . Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**. Per questa esercitazione non è necessaria l'autenticazione.

   ![Modello MVC nella finestra di dialogo nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. In **Esplora soluzioni**aprire il file *views/Home/index. cshtml*. Sostituire il codice in questo file con quanto segue:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Per la variabile *serviceUrl* , usare l'URI dell'app WebService.

3. Eseguire l'app WebClient localmente o pubblicarla in un altro sito Web.

Quando si fa clic sul pulsante "prova", viene inviata una richiesta AJAX all'app WebService usando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT). In questo modo è possibile esaminare diverse richieste tra origini. Attualmente, l'app WebService non supporta CORS, quindi se si fa clic sul pulsante si otterrà un errore.

![Errore ' try it ' nel browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Se si osserva il traffico HTTP in uno strumento come [Fiddler](https://www.telerik.com/fiddler), si noterà che il browser invia la richiesta GET e la richiesta ha esito positivo, ma la chiamata AJAX restituisce un errore. È importante comprendere che i criteri della stessa origine non impediscono al browser di *inviare* la richiesta. Ma impedisce all'applicazione di visualizzare la *risposta*.

![Debugger Web Fiddler che mostra le richieste Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Abilitare CORS

A questo punto è possibile abilitare CORS nell'app WebService. Per prima cosa, aggiungere il pacchetto NuGet CORS. In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**. Nella finestra console di gestione pacchetti digitare il comando seguente:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie API Web di base. Usare il flag di `-Version` per impostare come destinazione una versione specifica. Il pacchetto CORS richiede l'API Web 2,0 o versione successiva.

Aprire l' *app file\_Start/WebApiConfig. cs*. Aggiungere il codice seguente al metodo **WebApiConfig. Register** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Successivamente, aggiungere l'attributo **[EnableCors]** alla classe `TestController`:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Per il parametro *Origins* , usare l'URI in cui è stata distribuita l'applicazione WebClient. Ciò consente le richieste tra le origini da WebClient, pur non consentindo comunque tutte le altre richieste tra domini. In seguito verranno descritti in modo più dettagliato i parametri per **[EnableCors]** .

Non includere una barra alla fine dell'URL delle *origini* .

Ridistribuire l'applicazione WebService aggiornata. Non è necessario aggiornare WebClient. A questo punto la richiesta AJAX da WebClient dovrebbe avere esito positivo. Sono consentiti tutti i metodi GET, PUT e POST.

![Web browser visualizzazione del messaggio di prova riuscito](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Funzionamento di CORS

In questa sezione viene descritto cosa accade in una richiesta CORS, a livello dei messaggi HTTP. È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare correttamente l'attributo **[EnableCors]** e risolvere i problemi se le cose non funzionano come previsto.

La specifica CORS introduce diverse nuove intestazioni HTTP che consentono richieste tra le origini. Se un browser supporta CORS, queste intestazioni vengono impostate automaticamente per le richieste tra le origini. non è necessario eseguire alcuna operazione speciale nel codice JavaScript.

Di seguito è riportato un esempio di richiesta tra le origini. L'intestazione "Origin" fornisce il dominio del sito che sta effettuando la richiesta.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin. Il valore di questa intestazione corrisponde all'intestazione Origin o è il valore jolly "\*", che indica che è consentita un'origine.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX ha esito negativo. In particolare, il browser non consente la richiesta. Anche se il server restituisce una risposta con esito positivo, il browser non rende disponibile la risposta all'applicazione client.

**Richieste preliminari**

Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, denominata "richiesta preliminare", prima di inviare la richiesta effettiva per la risorsa.

Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:

- Il metodo di richiesta è GET, HEAD o POST *e*
- L'applicazione non imposta intestazioni di richiesta diverse da Accept, Accept-Language, Content-Language, Content-Type o Last-Event-ID *e*
- L'intestazione Content-Type (se impostata) è una delle seguenti:

    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain

La regola sulle intestazioni della richiesta si applica alle intestazioni impostate dall'applicazione chiamando **setRequestHeader** sull'oggetto **XMLHttpRequest** . (La specifica CORS chiama queste "intestazioni di richiesta autore"). La regola non si applica alle intestazioni che possono essere impostate dal *browser* , ad esempio l'agente utente, l'host o la lunghezza del contenuto.

Di seguito è riportato un esempio di una richiesta preliminare:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La richiesta pre-flight usa il metodo delle opzioni HTTP. Sono incluse due intestazioni speciali:

- Access-Control-request-method: il metodo HTTP che verrà usato per la richiesta effettiva.
- Access-Control-Request-Headers: elenco di intestazioni di richiesta impostate dall' *applicazione* per la richiesta effettiva. Anche in questo caso non sono incluse le intestazioni impostate dal browser.

Di seguito è riportato un esempio di risposta, presupponendo che il server consenta la richiesta:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La risposta include un'intestazione Access-Control-Allow-methods che elenca i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Allow-Headers, che elenca le intestazioni consentite. Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.

Gli strumenti comunemente usati per testare gli endpoint con le richieste di opzioni preliminari (ad esempio [Fiddler](https://www.telerik.com/fiddler) e [postazione](https://www.getpostman.com/)) non inviano le intestazioni delle opzioni richieste per impostazione predefinita. Verificare che le intestazioni `Access-Control-Request-Method` e `Access-Control-Request-Headers` vengano inviate con la richiesta e che le intestazioni delle opzioni raggiungano l'app tramite IIS.

Per configurare IIS per consentire a un'app ASP.NET di ricevere e gestire le richieste di opzioni, aggiungere la configurazione seguente al file *Web. config* dell'app nella sezione `<system.webServer><handlers>`:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

La rimozione di `OPTIONSVerbHandler` impedisce a IIS di gestire le richieste di opzioni. La sostituzione di `ExtensionlessUrlHandler-Integrated-4.0` consente alle richieste di opzioni di raggiungere l'app perché la registrazione dei moduli predefinita consente solo richieste GET, HEAD, POST e DEBUG con URL senza estensione.

## <a name="scope-rules-for-enablecors"></a>Regole di ambito per [EnableCors]

È possibile abilitare CORS per azione, per controller o a livello globale per tutti i controller API Web nell'applicazione.

**Per azione**

Per abilitare CORS per una singola azione, impostare l'attributo **[EnableCors]** nel metodo di azione. Nell'esempio seguente viene abilitato CORS solo per il metodo `GetItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Per controller**

Se si imposta **[EnableCors]** nella classe controller, si applica a tutte le azioni sul controller. Per disabilitare CORS per un'azione, aggiungere l'attributo **[DisableCors]** all'azione. Nell'esempio seguente viene abilitato CORS per ogni metodo eccetto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalmente**

Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un'istanza di **EnableCorsAttribute** al metodo **EnableCors** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Se si imposta l'attributo in più di un ambito, l'ordine di precedenza è:

1. Azione
2. Controller
3. Global

## <a name="set-the-allowed-origins"></a>Imposta le origini consentite

Il parametro *Origins* dell'attributo **[EnableCors]** specifica quali origini sono autorizzate ad accedere alla risorsa. Il valore è un elenco delimitato da virgole delle origini consentite.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

È anche possibile usare il valore jolly "\*" per consentire le richieste provenienti da qualsiasi origine.

Valutare attentamente prima di consentire le richieste provenienti da qualsiasi origine. Ciò significa che, letteralmente, qualsiasi sito Web può effettuare chiamate AJAX all'API Web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Impostare i metodi HTTP consentiti

Il parametro *Methods* dell'attributo **[EnableCors]** specifica quali metodi HTTP sono autorizzati ad accedere alla risorsa. Per consentire tutti i metodi, usare il valore jolly "\*". Nell'esempio seguente vengono consentite solo le richieste GET e POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Impostare le intestazioni della richiesta consentita

Questo articolo descrive in precedenza come una richiesta preliminare può includere un'intestazione Access-Control-Request-Headers, che elenca le intestazioni HTTP impostate dall'applicazione (le cosiddette "intestazioni della richiesta di autore"). Il parametro *headers* dell'attributo **[EnableCors]** specifica quali intestazioni della richiesta di autore sono consentite. Per consentire qualsiasi intestazione, impostare le *intestazioni* su "\*". Per includere intestazioni specifiche, impostare le *intestazioni* su un elenco delimitato da virgole delle intestazioni consentite:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Tuttavia, i browser non sono completamente coerenti nella modalità di impostazione di Access-Control-Request-Headers. Ad esempio, Chrome include attualmente "Origin". FireFox non include intestazioni standard, ad esempio "Accept", anche quando l'applicazione le imposta nello script.

Se si impostano le *intestazioni* su un valore diverso da "\*", è necessario includere almeno "Accept", "Content-Type" e "Origin", più eventuali intestazioni personalizzate che si desidera supportare.

## <a name="set-the-allowed-response-headers"></a>Impostare le intestazioni di risposta consentite

Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione. Le intestazioni di risposta disponibili per impostazione predefinita sono:

- Cache-Control
- Content-Language
- Content-Type
- Scadenza
- Ultima modifica
- Pragma

La specifica CORS chiama queste [semplici intestazioni di risposta](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Per rendere disponibili altre intestazioni per l'applicazione, impostare il parametro *exposedHeaders* di **[EnableCors]** .

Nell'esempio seguente, il metodo `Get` del controller imposta un'intestazione personalizzata denominata ' X-custom-header '. Per impostazione predefinita, il browser non esporrà questa intestazione in una richiesta tra le origini. Per rendere disponibile l'intestazione, includere ' X-custom-header ' in *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Passare le credenziali nelle richieste tra le origini

Le credenziali richiedono una gestione speciale in una richiesta CORS. Per impostazione predefinita, il browser non invia alcuna credenziale con una richiesta tra le origini. Le credenziali includono i cookie e gli schemi di autenticazione HTTP. Per inviare le credenziali con una richiesta tra le origini, il client deve impostare **XMLHttpRequest. withCredentials** su true.

Utilizzando direttamente **XMLHttpRequest** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Inoltre, il server deve consentire le credenziali. Per consentire le credenziali tra le origini nell'API Web, impostare la proprietà **SupportsCredentials** su true nell'attributo **[EnableCors]** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Se questa proprietà è true, la risposta HTTP includerà un'intestazione Access-Control-Allow-Credentials. Questa intestazione indica al browser che il server consente le credenziali per una richiesta tra le origini.

Se il browser invia le credenziali, ma la risposta non include un'intestazione Access-Control-Allow-Credentials valida, il browser non esporrà la risposta all'applicazione e la richiesta AJAX avrà esito negativo.

Prestare attenzione quando si imposta **SupportsCredentials** su true, in quanto significa che un sito Web in un altro dominio può inviare le credenziali di un utente connesso all'API Web per conto dell'utente, senza che l'utente sia in grado di riconoscere. La specifica CORS indica anche che l'impostazione delle *origini* per &quot;\*&quot; non è valida se **SupportsCredentials** è true.

## <a name="custom-cors-policy-providers"></a>Provider di criteri CORS personalizzati

L'attributo **[EnableCors]** implementa l'interfaccia **ICorsPolicyProvider** . È possibile fornire un'implementazione personalizzata creando una classe che deriva da **attribute** e implementa **ICorsPolicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

A questo punto è possibile applicare l'attributo a qualsiasi posizione da inserire **[EnableCors]** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Un provider di criteri CORS personalizzato, ad esempio, potrebbe leggere le impostazioni da un file di configurazione.

In alternativa all'uso degli attributi, è possibile registrare un oggetto **ICorsPolicyProviderFactory** che crea oggetti **ICorsPolicyProvider** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Per impostare **ICorsPolicyProviderFactory**, chiamare il metodo di estensione **SetCorsPolicyProviderFactory** all'avvio, come indicato di seguito:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Supporto browser

Il pacchetto CORS dell'API Web è una tecnologia lato server. Il browser dell'utente deve anche supportare CORS. Fortunatamente, le versioni correnti di tutti i principali browser includono il [supporto per CORS](http://caniuse.com/cors).
