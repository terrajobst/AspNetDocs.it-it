---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Elaborazione delle eccezioni non gestite (VB) | Microsoft Docs
author: rick-anderson
description: Quando si verifica un errore di runtime in un'applicazione Web nell'ambiente di produzione, è importante informare uno sviluppatore e registrare l'errore in modo che possa essere diagnosticato in una...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d2e12af29bd95ce9898fa6a5e81be8b7a761f76
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586065"
---
# <a name="processing-unhandled-exceptions-vb"></a>Elaborazione delle eccezioni non gestite (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([procedura per il download](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Quando si verifica un errore di runtime in un'applicazione Web nell'ambiente di produzione, è importante informare uno sviluppatore e registrare l'errore in modo che possa essere diagnosticato in un momento successivo. Questa esercitazione offre una panoramica del modo in cui ASP.NET elabora gli errori di runtime e analizza un modo per eseguire il codice personalizzato ogni volta che un'eccezione non gestita si propaga al runtime di ASP.NET.

## <a name="introduction"></a>Introduzione

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, si esegue il bubbling fino al runtime di ASP.NET, che genera l'evento `Error` e visualizza la pagina di errore appropriata. Sono disponibili tre tipi diversi di pagine di errore: lo schermo di errore di runtime giallo (YSOD); Dettagli eccezione YSOD; e pagine di errore personalizzate. Nell' [esercitazione precedente](displaying-a-custom-error-page-vb.md) è stata configurata l'applicazione per usare una pagina di errore personalizzata per gli utenti remoti e i dettagli dell'eccezione YSOD per gli utenti che visitano localmente.

L'uso di una pagina di errore personalizzata che corrisponde all'aspetto del sito è preferibile all'errore di runtime predefinito YSOD, ma la visualizzazione di una pagina di errore personalizzata è solo una parte di una soluzione completa di gestione degli errori. Quando si verifica un errore in un'applicazione nell'ambiente di produzione, è importante che gli sviluppatori ricevano la notifica dell'errore, in modo che possano scoprire la ragione dell'eccezione e risolverla. Inoltre, è necessario registrare i dettagli dell'errore in modo che l'errore possa essere esaminato e diagnosticato in un secondo momento.

Questa esercitazione illustra come accedere ai dettagli di un'eccezione non gestita in modo che possano essere registrati e uno sviluppatore abbia inviato una notifica. Le due esercitazioni successive esaminano le librerie di registrazione degli errori che, dopo un po' di configurazione, invieranno automaticamente notifiche agli sviluppatori di errori di runtime e registreranno i dettagli.

> [!NOTE]
> Le informazioni esaminate in questa esercitazione sono particolarmente utili se è necessario elaborare le eccezioni non gestite in modo univoco o personalizzato. Nei casi in cui è necessario registrare l'eccezione e inviare una notifica a uno sviluppatore, è possibile usare una libreria di registrazione degli errori. Le due esercitazioni successive forniscono una panoramica di due librerie di questo tipo.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Esecuzione del codice quando viene generato l'evento`Error`

Gli eventi forniscono a un oggetto un meccanismo per segnalare che si è verificato un evento interessante e che un altro oggetto può eseguire il codice in risposta. Gli sviluppatori ASP.NET sono abituati a pensare in termini di eventi. Se si vuole eseguire codice quando il visitatore fa clic su un pulsante particolare, si crea un gestore eventi per l'evento `Click` del pulsante e si inserisce il codice. Dato che il runtime di ASP.NET genera l' [evento`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) ogni volta che si verifica un'eccezione non gestita, il codice per la registrazione dei dettagli dell'errore verrebbe inserito in un gestore eventi. Ma come si crea un gestore eventi per l'evento `Error`?

L'evento `Error` è uno dei numerosi eventi nella [classe`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) generati in determinate fasi della pipeline HTTP durante il ciclo di vita di una richiesta. Ad esempio, l' [evento`BeginRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) della classe `HttpApplication` viene generato all'inizio di ogni richiesta; il relativo [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) viene generato quando un modulo di sicurezza ha identificato il richiedente. Questi eventi `HttpApplication` offrono allo sviluppatore della pagina un mezzo per eseguire la logica personalizzata nei vari punti della durata di una richiesta.

I gestori eventi per gli eventi di `HttpApplication` possono essere inseriti in un file speciale denominato `Global.asax`. Per creare questo file nel sito Web, aggiungere un nuovo elemento alla radice del sito Web usando il modello di classe dell'applicazione globale con il nome `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figura 1**: aggiungere `Global.asax` all'applicazione Web  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](processing-unhandled-exceptions-vb/_static/image3.png))

Il contenuto e la struttura del file di `Global.asax` creato da Visual Studio differiscono leggermente a seconda che si usi un progetto di applicazione Web (WAP) o un progetto di sito Web (WSP). Con un WAP, il `Global.asax` viene implementato come due file distinti `Global.asax` e `Global.asax.vb`. Il file di `Global.asax` non contiene elementi, ma una direttiva `@Application` che fa riferimento al file di `.vb`; i gestori di eventi di interesse vengono definiti nel file di `Global.asax.vb`. Per WSPs, viene creato un solo file, `Global.asax`e i gestori eventi sono definiti in un blocco di `<script runat="server">`.

Il file di `Global.asax` creato in un modello di classe di applicazione globale del WAP da Visual Studio include i gestori eventi denominati `Application_BeginRequest`, `Application_AuthenticateRequest`e `Application_Error`, ovvero i gestori eventi per gli eventi `HttpApplication` `BeginRequest`, `AuthenticateRequest`e `Error`rispettivamente. Sono inoltre presenti gestori eventi denominati `Application_Start`, `Session_Start`, `Application_End`e `Session_End`, ovvero gestori eventi che vengono attivati all'avvio dell'applicazione Web, quando viene avviata una nuova sessione, al termine dell'applicazione e al termine di una sessione. Il file di `Global.asax` creato in un oggetto WSP da Visual Studio contiene solo i gestori eventi `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`e `Session_End`.

> [!NOTE]
> Quando si distribuisce l'applicazione ASP.NET, è necessario copiare il file di `Global.asax` nell'ambiente di produzione. Il file di `Global.asax.vb`, creato nel WAP, non deve essere copiato in produzione perché questo codice viene compilato nell'assembly del progetto.

I gestori eventi creati dal modello della classe di applicazione globale di Visual Studio non sono esaustivi. È possibile aggiungere un gestore eventi per qualsiasi evento di `HttpApplication` assegnando un nome al gestore eventi `Application_EventName`. Ad esempio, è possibile aggiungere il codice seguente al file di `Global.asax` per creare un gestore eventi per l' [evento`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Analogamente, è possibile rimuovere qualsiasi gestore eventi creato dal modello di classe dell'applicazione globale che non è necessario. Per questa esercitazione è necessario solo un gestore eventi per l'evento `Error`; è possibile rimuovere gli altri gestori eventi dal file di `Global.asax`.

> [!NOTE]
> I *moduli HTTP* offrono un altro modo per definire i gestori eventi per `HttpApplication` eventi. I moduli HTTP vengono creati come file di classe che possono essere inseriti direttamente nel progetto di applicazione Web o separati in una libreria di classi separata. Poiché possono essere separate in una libreria di classi, i moduli HTTP offrono un modello più flessibile e riutilizzabile per la creazione di `HttpApplication` gestori di eventi. Mentre il file di `Global.asax` è specifico dell'applicazione Web in cui risiede, i moduli HTTP possono essere compilati in assembly. a questo punto, l'aggiunta del modulo HTTP a un sito Web è semplice come eliminare l'assembly nella cartella `Bin` e registrare il modulo in `Web.config`. Questa esercitazione non illustra la creazione e l'uso di moduli HTTP, ma le due librerie di registrazione degli errori usate nelle due esercitazioni seguenti sono implementate come moduli HTTP. Per altre informazioni sui vantaggi dei moduli HTTP [, vedere uso di moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Recupero delle informazioni sull'eccezione non gestita

A questo punto è presente un file Global. asax con un gestore dell'evento `Application_Error`. Quando questo gestore dell'evento viene eseguito, è necessario notificare allo sviluppatore l'errore e registrarne i dettagli. Per eseguire queste attività, è necessario innanzitutto determinare i dettagli dell'eccezione generata. Utilizzare il [metodo di`GetLastError`](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) dell'oggetto server per recuperare i dettagli dell'eccezione non gestita che ha causato l'attivazione dell'evento `Error`.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Il metodo `GetLastError` restituisce un oggetto di tipo `Exception`, che è il tipo di base per tutte le eccezioni nella .NET Framework. Tuttavia, nel codice precedente si sta eseguendo il cast dell'oggetto eccezione restituito da `GetLastError` in un oggetto `HttpException`. Se l'evento `Error` viene generato perché è stata generata un'eccezione durante l'elaborazione di una risorsa ASP.NET, l'eccezione generata viene inserita all'interno di un `HttpException`. Per ottenere l'eccezione effettiva che ha provocato l'evento Error, utilizzare la proprietà `InnerException`. Se l'evento `Error` è stato generato a causa di un'eccezione basata su HTTP, ad esempio una richiesta di una pagina inesistente, viene generata una `HttpException`, ma non è presente un'eccezione interna.

Il codice seguente usa il `GetLastErrormessage` per recuperare informazioni sull'eccezione che ha attivato l'evento `Error`, archiviando il `HttpException` in una variabile denominata `lastErrorWrapper`. Archivia quindi il tipo, il messaggio e la traccia dello stack dell'eccezione di origine in tre variabili stringa, verificando se la `lastErrorWrapper` è l'eccezione effettiva che ha generato l'evento `Error` (nel caso di eccezioni basate su HTTP) o se si tratta semplicemente di un wrapper per un'eccezione generata durante l'elaborazione della richiesta.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

A questo punto sono disponibili tutte le informazioni necessarie per scrivere codice che registrerà i dettagli dell'eccezione in una tabella di database. È possibile creare una tabella di database con colonne per tutti i dettagli dell'errore di interesse, ovvero il tipo, il messaggio, l'analisi dello stack e così via, insieme ad altre informazioni utili, ad esempio l'URL della pagina richiesta e il nome dell'utente attualmente connesso. Nel gestore dell'evento `Application_Error` connettersi al database e inserire un record nella tabella. Analogamente, è possibile aggiungere codice per avvisare lo sviluppatore dell'errore tramite posta elettronica.

Le librerie di registrazione degli errori esaminate nelle due esercitazioni successive forniscono la funzionalità predefinita, quindi non è necessario compilare manualmente la registrazione e la notifica degli errori. Tuttavia, per illustrare che è in corso la generazione dell'evento `Error` e che il gestore dell'evento `Application_Error` può essere usato per registrare i dettagli dell'errore e inviare una notifica a uno sviluppatore, aggiungere il codice che notifica a uno sviluppatore quando si verifica un errore.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notifica a uno sviluppatore quando si verifica un'eccezione non gestita

Quando si verifica un'eccezione non gestita nell'ambiente di produzione, è importante informare il team di sviluppo in modo che sia in grado di valutare l'errore e determinare quali azioni devono essere eseguite. Se, ad esempio, si verifica un errore durante la connessione al database, sarà necessario controllare la stringa di connessione e, forse, aprire un ticket di supporto con la società di hosting Web. Se si è verificata un'eccezione a causa di un errore di programmazione, potrebbe essere necessario aggiungere codice aggiuntivo o logica di convalida per evitare tali errori in futuro.

Le classi .NET Framework nello [spazio dei nomi`System.Net.Mail`](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitano l'invio di un messaggio di posta elettronica. La [classe`MailMessage`](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) rappresenta un messaggio di posta elettronica con proprietà quali `To`, `From`, `Subject`, `Body`e `Attachments`. Il `SmtpClass` viene utilizzato per inviare un oggetto `MailMessage` utilizzando un server SMTP specificato. le impostazioni del server SMTP possono essere specificate a livello di programmazione o in modo dichiarativo nell' [elemento`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx) del `Web.config file`. Per altre informazioni sull'invio di messaggi di posta elettronica in un'applicazione ASP.NET, vedere l'articolo relativo all' [invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e alle [domande frequenti su System .NET. mail](http://systemnetmail.com/).

> [!NOTE]
> L'elemento `<system.net>` contiene le impostazioni del server SMTP utilizzate dalla classe `SmtpClient` quando si invia un messaggio di posta elettronica. È probabile che la società di hosting Web disponga di un server SMTP che è possibile usare per inviare messaggi di posta elettronica dall'applicazione. Per informazioni sulle impostazioni del server SMTP da utilizzare nell'applicazione Web, consultare la sezione supporto dell'host Web.

Aggiungere il codice seguente al gestore eventi `Application_Error` per inviare a uno sviluppatore un messaggio di posta elettronica quando si verifica un errore:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Sebbene il codice precedente sia abbastanza lungo, la maggior parte di esso crea il codice HTML visualizzato nel messaggio di posta elettronica inviato allo sviluppatore. Il codice inizia facendo riferimento al `HttpException` restituito dal metodo `GetLastError` (`lastErrorWrapper`). L'eccezione effettiva generata dalla richiesta viene recuperata tramite `lastErrorWrapper.InnerException` e viene assegnata alla variabile `lastError`. Le informazioni sul tipo, il messaggio e la traccia dello stack vengono recuperate da `lastError` e archiviate in tre variabili di stringa.

Viene quindi creato un oggetto `MailMessage` denominato `mm`. Il corpo del messaggio di posta elettronica è formattato in formato HTML e visualizza l'URL della pagina richiesta, il nome dell'utente attualmente connesso e le informazioni sull'eccezione (tipo, messaggio e traccia dello stack). Una delle cose più interessanti sulla classe `HttpException` è che è possibile generare il codice HTML usato per creare la schermata gialla della morte (YSOD) dei dettagli dell'eccezione chiamando il [Metodo GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Questo metodo viene usato qui per recuperare i dettagli dell'eccezione YSOD markup e aggiungerlo al messaggio di posta elettronica come allegato. Una parola di attenzione: se l'eccezione che ha generato l'evento `Error` era un'eccezione basata su HTTP, ad esempio una richiesta per una pagina inesistente, il metodo `GetHtmlErrorMessage` restituirà `null`.

Il passaggio finale consiste nell'inviare il `MailMessage`. Questa operazione viene eseguita creando un nuovo metodo `SmtpClient` e chiamando il relativo metodo `Send`.

> [!NOTE]
> Prima di usare questo codice nell'applicazione Web, è necessario modificare i valori nel `ToAddress` e `FromAddress` costanti da support@example.com a qualsiasi indirizzo di posta elettronica a cui inviare il messaggio di notifica di errore e che provengono da. È inoltre necessario specificare le impostazioni del server SMTP nella sezione `<system.net>` `Web.config`. Per determinare le impostazioni del server SMTP da utilizzare, consultare il provider dell'host Web.

Con questo codice quando si verifica un errore, lo sviluppatore riceve un messaggio di posta elettronica che riepiloga l'errore e include YSOD. Nell'esercitazione precedente è stato illustrato un errore di runtime visitando Genre. aspx e passando un valore di `ID` non valido attraverso la QueryString, ad esempio `Genre.aspx?ID=foo`. Visitare la pagina con il file di `Global.asax` sul posto produce la stessa esperienza utente dell'esercitazione precedente. nell'ambiente di sviluppo, si continuerà a visualizzare la schermata gialla Dettagli eccezione, mentre nell'ambiente di produzione verrà visualizzata la pagina di errore personalizzata. Oltre a questo comportamento esistente, lo sviluppatore riceve un messaggio di posta elettronica.

La **Figura 2** illustra il messaggio di posta elettronica ricevuto durante la visita `Genre.aspx?ID=foo`. Il corpo del messaggio riepiloga le informazioni sull'eccezione, mentre l'allegato `YSOD.htm` Visualizza il contenuto illustrato nei dettagli dell'eccezione YSOD (vedere la **Figura 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figura 2**: lo sviluppatore riceve una notifica di posta elettronica ogni volta che si verifica un'eccezione non gestita  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figura 3**: la notifica tramite posta elettronica include i dettagli dell'eccezione YSOD come allegato  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Che cosa si sta usando la pagina di errore personalizzata?

In questa esercitazione è stato illustrato come utilizzare `Global.asax` e il gestore dell'evento `Application_Error` per eseguire il codice quando si verifica un'eccezione non gestita. In particolare, questo gestore eventi è stato usato per notificare a uno sviluppatore un errore. è possibile estenderlo per registrare anche i dettagli dell'errore in un database. La presenza del gestore dell'evento `Application_Error` non influisce sull'esperienza dell'utente finale. Visualizzano comunque la pagina di errore configurata, i dettagli dell'errore YSOD, l'errore di runtime YSOD o la pagina di errore personalizzata.

È naturale chiedersi se il file `Global.asax` e l'evento `Application_Error` sono necessari quando si usa una pagina di errore personalizzata. Quando si verifica un errore, l'utente Visualizza la pagina di errore personalizzata, quindi perché non è possibile inserire il codice per inviare una notifica allo sviluppatore e registrare i dettagli dell'errore nella classe code-behind della pagina di errore personalizzata? Sebbene sia certamente possibile aggiungere codice alla classe code-behind della pagina di errore personalizzata, non è possibile accedere ai dettagli dell'eccezione che ha attivato l'evento `Error` quando si usa la tecnica esplorata nell'esercitazione precedente. La chiamata del metodo `GetLastError` dalla pagina di errore personalizzata restituisce `Nothing`.

Il motivo di questo comportamento è dovuto al fatto che la pagina di errore personalizzata viene raggiunta tramite un reindirizzamento. Quando un'eccezione non gestita raggiunge il runtime ASP.NET, il motore ASP.NET genera il relativo evento `Error` (che esegue il gestore dell'evento `Application_Error`) e quindi *reindirizza* l'utente alla pagina di errore personalizzata emettendo un `Response.Redirect(customErrorPageUrl)`. Il metodo `Response.Redirect` invia una risposta al client con un codice di stato HTTP 302, indicando al browser di richiedere un nuovo URL, ovvero la pagina di errore personalizzata. Il browser richiede quindi automaticamente la nuova pagina. È possibile indicare che la pagina di errore personalizzata è stata richiesta separatamente dalla pagina in cui ha avuto origine l'errore perché la barra degli indirizzi del browser viene modificata nell'URL della pagina di errore personalizzata (vedere la **Figura 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figura 4**: quando si verifica un errore, il browser viene reindirizzato all'URL della pagina di errore personalizzata  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](processing-unhandled-exceptions-vb/_static/image12.png))

Il risultato finale è che la richiesta in cui si è verificata l'eccezione non gestita termina quando il server risponde con il reindirizzamento HTTP 302. La richiesta successiva alla pagina di errore personalizzata è una nuova richiesta. a questo punto il motore ASP.NET ha ignorato le informazioni sull'errore e, inoltre, non ha modo di associare l'eccezione non gestita nella richiesta precedente con la nuova richiesta per la pagina di errore personalizzata. Questo è il motivo per cui `GetLastError` restituisce `null` quando viene chiamato dalla pagina di errore personalizzata.

Tuttavia, è possibile che la pagina di errore personalizzata venga eseguita durante la stessa richiesta che ha causato l'errore. Il metodo [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) trasferisce l'esecuzione all'URL specificato e lo elabora all'interno della stessa richiesta. È possibile spostare il codice nel gestore eventi `Application_Error` nella classe code-behind della pagina di errore personalizzata, sostituendo tale codice in `Global.asax` con il codice seguente:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

A questo punto, quando si verifica un'eccezione non gestita, il gestore eventi `Application_Error` trasferisce il controllo alla pagina di errore personalizzata appropriata in base al codice di stato HTTP. Poiché il controllo è stato trasferito, la pagina di errore personalizzata ha accesso alle informazioni sulle eccezioni non gestite tramite `Server.GetLastError` e può inviare una notifica a uno sviluppatore dell'errore e registrarne i dettagli. La chiamata `Server.Transfer` impedisce al motore ASP.NET di reindirizzare l'utente alla pagina di errore personalizzata. Al contrario, il contenuto della pagina di errore personalizzata viene restituito come risposta alla pagina che ha generato l'errore.

## <a name="summary"></a>Riepilogo

Quando si verifica un'eccezione non gestita in un'applicazione Web ASP.NET, il runtime di ASP.NET genera l'evento `Error` e visualizza la pagina di errore configurata. È possibile notificare allo sviluppatore l'errore, registrarne i dettagli o elaborarlo in altro modo, creando un gestore eventi per l'evento di errore. Esistono due modi per creare un gestore eventi per `HttpApplication` eventi come `Error`: nel file di `Global.asax` o da un modulo HTTP. In questa esercitazione è stato illustrato come creare un gestore dell'evento `Error` nel file di `Global.asax` che informa gli sviluppatori di un errore per mezzo di un messaggio di posta elettronica.

La creazione di un gestore eventi `Error` è utile se è necessario elaborare le eccezioni non gestite in modo univoco o personalizzato. Tuttavia, la creazione di un gestore dell'evento `Error` per registrare l'eccezione o per inviare una notifica a uno sviluppatore non è il modo più efficiente del tempo poiché esiste già una raccolta di log degli errori gratuita e facile da usare che può essere configurata in pochi minuti. Le due esercitazioni successive esaminano due librerie di questo tipo.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Cenni preliminari sui moduli HTTP ASP.NET e sui gestori HTTP](https://support.microsoft.com/kb/307985)
- [Risposta normale a eccezioni non gestite-elaborazione di eccezioni non gestite](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [Classe `HttpApplication` e oggetto applicazione ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Gestori HTTP e moduli HTTP in ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Invio di messaggi di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Informazioni sul file di `Global.asax`](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Uso di moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Uso del file di `Global.asax` ASP.NET](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Utilizzo delle istanze di `HttpApplication`](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Precedente](displaying-a-custom-error-page-vb.md)
> [Successivo](logging-error-details-with-asp-net-health-monitoring-vb.md)
