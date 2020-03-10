---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevenzione degli attacchi di reindirizzamento aperti (C#) | Microsoft Docs
author: jongalloway
description: Questa esercitazione illustra come impedire gli attacchi di reindirizzamento aperti nelle applicazioni MVC ASP.NET. In questa esercitazione vengono illustrate le modifiche apportate...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538171"
---
# <a name="preventing-open-redirection-attacks-c"></a>Prevenzione di attacchi di reindirizzamento aperti (C#)

di [Jon Galloway](https://github.com/jongalloway)

> Questa esercitazione illustra come impedire gli attacchi di reindirizzamento aperti nelle applicazioni MVC ASP.NET. In questa esercitazione vengono illustrate le modifiche apportate in AccountController in ASP.NET MVC 3 e viene illustrato come è possibile applicare queste modifiche nelle applicazioni ASP.NET MVC 1,0 e 2 esistenti.

## <a name="what-is-an-open-redirection-attack"></a>Che cos'è un attacco di reindirizzamento aperto?

Qualsiasi applicazione Web che reindirizza a un URL specificato tramite la richiesta, ad esempio la QueryString o i dati del modulo, può essere potenzialmente manomessa per reindirizzare gli utenti a un URL esterno e dannoso. Questa manomissione è detta attacco di reindirizzamento aperto.

Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non sia stato alterato. L'account di accesso utilizzato nei AccountController predefiniti sia per ASP.NET MVC 1,0 che per ASP.NET MVC 2 è vulnerabile agli attacchi di reindirizzamento aperti. Fortunatamente, è facile aggiornare le applicazioni esistenti per usare le correzioni di ASP.NET MVC 3 Preview.

Per comprendere la vulnerabilità, è possibile esaminare il funzionamento del reindirizzamento degli account di accesso in un progetto di applicazione Web MVC 2 ASP.NET predefinito. In questa applicazione, il tentativo di visitare un'azione del controller con l'attributo [autorizzate] reindirizza gli utenti non autorizzati alla vista/Account/LogOn. Questo reindirizzamento a/Account/LogOn includerà un parametro QueryString returnUrl, in modo che l'utente possa essere restituito all'URL richiesto in origine dopo che è stato effettuato l'accesso.

Nella schermata seguente si può notare che un tentativo di accedere alla visualizzazione/Account/ChangePassword quando non è stato eseguito l'accesso comporta un reindirizzamento a/Account/LogOn? ReturnUrl =% 2fAccount% 2fChangePassword% 2F.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: pagina di accesso con reindirizzamento aperto

Poiché il parametro QueryString ReturnUrl non viene convalidato, un utente malintenzionato può modificarlo per inserire qualsiasi indirizzo URL nel parametro per condurre un attacco di reindirizzamento aperto. Per illustrare questo problema, è possibile modificare il parametro ReturnUrl in [http://bing.com](http://bing.com), quindi l'URL di accesso risultante sarà/account/logon? ReturnUrl =<http://www.bing.com/>. Una volta eseguito correttamente l'accesso al sito, si verrà reindirizzati a [http://bing.com](http://bing.com). Poiché questo reindirizzamento non viene convalidato, può puntare a un sito dannoso che tenta di ingannare l'utente.

### <a name="a-more-complex-open-redirection-attack"></a>Un attacco di reindirizzamento aperto più complesso

Gli attacchi di reindirizzamento aperti sono particolarmente pericolosi perché un utente malintenzionato sa che stiamo tentando di accedere a un sito Web specifico, che ci rende vulnerabili a un [attacco di phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Ad esempio, un utente malintenzionato potrebbe inviare messaggi di posta elettronica dannosi agli utenti del sito Web durante un tentativo di acquisizione delle password. Verrà ora esaminato il funzionamento del sito NerdDinner. Si noti che il sito Live NerdDinner è stato aggiornato per proteggersi da attacchi di reindirizzamento aperti.

In primo luogo, un utente malintenzionato invia un collegamento alla pagina di accesso in NerdDinner che include un reindirizzamento alla pagina falsificata:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Si noti che l'URL restituito punta a nerddiner.com, che manca un "n" dalla parola dinner. In questo esempio, si tratta di un dominio che l'utente malintenzionato controlla. Quando si accede al collegamento precedente, viene eseguita la pagina di accesso legittimo a NerdDinner.com.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: pagina di accesso di NerdDinner con reindirizzamento aperto

Quando si esegue correttamente l'accesso, l'azione di accesso di ASP.NET MVC AccountController reindirizza all'URL specificato nel parametro QueryString returnUrl. In questo caso, si tratta dell'URL immesso dall'utente malintenzionato, che è [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). A meno che non sia estremamente vigile, è molto probabile che non si noti, soprattutto perché l'autore dell'attacco ha fatto attenzione a verificare che la pagina falsificata appaia esattamente come la pagina di accesso legittima. Questa pagina di accesso include un messaggio di errore che richiede di eseguire di nuovo l'accesso. È necessario che la password sia stata digitata in forma non consentita.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: schermata di accesso NerdDinner contraffatta

Quando si digita di nuovo il nome utente e la password, la pagina di accesso falsificata Salva le informazioni e invia nuovamente il sito di NerdDinner.com legittimo. A questo punto, il sito di NerdDinner.com ha già eseguito l'autenticazione, quindi la pagina di accesso contraffatta può reindirizzare direttamente a tale pagina. Il risultato finale è che l'autore dell'attacco ha il nome utente e la password e non è a conoscenza della loro disposizione.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Esame del codice vulnerabile nell'azione di accesso AccountController

Il codice per l'azione di accesso in un'applicazione ASP.NET MVC 2 è illustrato di seguito. Si noti che dopo un accesso riuscito, il controller restituisce un reindirizzamento a returnUrl. È possibile osservare che non viene eseguita alcuna convalida sul parametro returnUrl.

**Listato 1-ASP.NET MVC 2-azione di accesso in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Verranno ora esaminate le modifiche apportate all'operazione di accesso a ASP.NET MVC 3. Questo codice è stato modificato per convalidare il parametro returnUrl chiamando un nuovo metodo nella classe helper System. Web. Mvc. URL denominata `IsLocalUrl()`.

**Listato 2: azione di accesso ASP.NET MVC 3 in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Questa operazione è stata modificata per convalidare il parametro Return URL chiamando un nuovo metodo nella classe helper System. Web. Mvc. URL, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protezione delle applicazioni ASP.NET MVC 1,0 e MVC 2

È possibile sfruttare le modifiche apportate a ASP.NET MVC 3 nelle applicazioni ASP.NET MVC 1,0 e 2 esistenti aggiungendo il metodo helper IsLocalUrl () e aggiornando l'azione LogOn per convalidare il parametro returnUrl.

Il Metodo UrlHelper IsLocalUrl () esegue semplicemente una chiamata a un metodo in System. Web. WebPages, in quanto questa convalida viene usata anche dalle applicazioni Pagine Web ASP.NET.

**Listato 3: il Metodo IsLocalUrl () da ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Il metodo IsUrlLocalToHost contiene la logica di convalida effettiva, come illustrato nel listato 4.

**Listato 4-Metodo IsUrlLocalToHost () dalla classe System. Web. WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Nell'applicazione ASP.NET MVC 1,0 o 2 viene aggiunto un metodo IsLocalUrl () a AccountController, ma è consigliabile aggiungerlo a una classe helper separata, se possibile. Si apporteranno due piccole modifiche alla versione ASP.NET MVC 3 di IsLocalUrl () in modo che funzioni all'interno di AccountController. In primo luogo, verrà modificato da un metodo pubblico a un metodo privato, perché i metodi pubblici nei controller sono accessibili come azioni del controller. In secondo luogo, si modificherà la chiamata che controlla l'host dell'URL rispetto all'host dell'applicazione. Tale chiamata utilizza un campo RequestContext locale nella classe UrlHelper. Anziché utilizzare questo. RequestContext. HttpContext. Request. URL. host, questa operazione verrà utilizzata. Request. URL. host. Il codice seguente illustra il Metodo IsLocalUrl () modificato da usare con una classe controller in ASP.NET MVC 1,0 e 2 Applications.

**Listato 5-Metodo IsLocalUrl (), che viene modificato per l'uso con una classe controller MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ora che il Metodo IsLocalUrl () è disponibile, è possibile chiamarlo dall'azione LogOn per convalidare il parametro returnUrl, come illustrato nel codice seguente.

**Elenco 6: metodo di accesso aggiornato che convalida il parametro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

A questo punto è possibile testare un attacco di reindirizzamento aperto provando ad accedere usando un URL restituito esterno. Si userà/Account/LogOn? ReturnUrl = di nuovo<http://www.bing.com/>.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: test dell'azione di accesso aggiornata

Dopo aver eseguito l'accesso, si verrà reindirizzati all'azione del controller Home/index, anziché all'URL esterno.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: attacco di reindirizzamento aperto sconfitto

## <a name="summary"></a>Riepilogo

Gli attacchi di reindirizzamento aperti possono verificarsi quando gli URL di reindirizzamento vengono passati come parametri nell'URL per un'applicazione. Il modello ASP.NET MVC 3 include il codice per la protezione da attacchi di reindirizzamento aperti. È possibile aggiungere questo codice con alcune modifiche alle applicazioni ASP.NET MVC 1,0 e 2. Per proteggersi da attacchi di reindirizzamento aperti durante l'accesso alle applicazioni ASP.NET 1,0 e 2, aggiungere un metodo IsLocalUrl () e convalidare il parametro returnUrl nell'azione LogOn.
