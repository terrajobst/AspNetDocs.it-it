---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guida alla risoluzione dei problemi di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive i problemi che potrebbero verificarsi quando si lavora con Pagine Web ASP.NET (Razor) e alcune soluzioni suggerite. Versioni del software ASP.NET Web pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585764"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive i problemi che potrebbero verificarsi quando si lavora con Pagine Web ASP.NET (Razor) e alcune soluzioni suggerite.
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2 e Pagine Web ASP.NET 1,0.

Questo argomento è suddiviso nelle sezioni seguenti:

- [Problemi con le pagine in esecuzione](#Issues_Running_.cshtml_Pages)
- [Problemi con il codice Razor](#IssuesWithRazorCode)
- [Problemi relativi alla sicurezza e all'appartenenza](#membership)
- [Problemi relativi all'invio di posta elettronica](#email)
- [Risorse aggiuntive](#AdditionalResources)

Per domande di carattere generale, vedere domande [frequenti su pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemi con le pagine in esecuzione

Una serie di problemi può impedire la corretta esecuzione delle pagine *. cshtml* e *. vbhtml* . In questa sezione vengono elencati i messaggi di errore comuni e le possibili cause.

### <a name="http-error-403---forbidden-access-is-denied"></a>Errore HTTP 403-accesso negato: accesso negato

*Non si dispone delle autorizzazioni necessarie per visualizzare la directory o la pagina utilizzando le credenziali fornite.*

Questo errore può verificarsi se il server non esegue la versione corretta del .NET Framework. Verificare che nel computer in cui è in esecuzione il server (in locale o in remoto) sia installato almeno il .NET Framework 4. Assicurarsi inoltre che l'applicazione stessa sia configurata per eseguire la versione corretta.

Se questo problema si verifica localmente durante l'uso di WebMatrix, fare clic sull'area di lavoro del **sito** e quindi in TreeView fare clic su **Impostazioni**. Nell'elenco **Seleziona versione .NET Framework** selezionare **.NET 4 (integrato)** . Se questa versione è già impostata, provare a eseguire WebMatrix come amministratore.

Verificare che nella radice del sito Web sia presente almeno un file con *estensione cshtml* .

Se questo errore viene visualizzato quando il server Web si trova in un server remoto, rivolgersi all'amministratore del server. Verificare che nel server sia installato il .NET Framework 4 o versione successiva. Assicurarsi inoltre che l'applicazione sia in esecuzione in un pool di applicazioni configurato per l'uso di tale versione di the.NET Framework.

Se si ha il controllo sul server, verificare che sia in esecuzione la versione corretta del .NET Framework. È anche possibile provare a ripristinare l'installazione eseguendo il comando `aspnet_regiis -iru`. Se, ad esempio, si installa IIS dopo l'installazione del .NET Framework, IIS non verrà configurato correttamente per l'esecuzione delle pagine ASP.NET. Per ulteriori informazioni, vedere [ASP.NET IIS Registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Errore HTTP 403,14-accesso negato

*Il server Web è configurato in modo da non elencare il contenuto di questa directory.*

Questo errore può verificarsi se si richiede una risorsa protetta, ad esempio il file *Web. config* , o che si trova in una cartella protetta (ad esempio, *dati di app\_* o *codice di\_dell'app*).

### <a name="http-error-40417---not-found"></a>Errore HTTP 404,17-non trovato

*Il contenuto richiesto sembra essere uno script e non verrà servito dal gestore di file statici.*

Questo errore può verificarsi se il server non è configurato correttamente per l'utilizzo di .NET Framework 4 o versione successiva e pertanto non riconosce il codice nei blocchi di `@{ }`. Vedere la descrizione precedente per l' *errore HTTP 403-accesso negato: accesso negato*.

### <a name="http-error-4047---not-found"></a>Errore HTTP 404,7-non trovato

*Il modulo filtro richieste è configurato per negare l'estensione di file*

Questo errore può verificarsi se le estensioni *. cshtml* o *. vbhtml* sono state bloccate in modo esplicito nel server. Un sintomo di questo problema è che gli URL funzionano quando non includono l'estensione, ma gli URL che includono *. cshtml* o *. vbhtml* non funzionano. Una possibile soluzione consiste nel riabilitare le estensioni nel file *Web. config* del sito. Nell'esempio seguente viene illustrato come abilitare l'estensione *cshtml* .

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Errore HTTP 404,8-non trovato

*Il modulo filtro richieste è configurato per negare un percorso nell'URL che contiene una sezione hiddenSegment.*

Questo errore può verificarsi se si richiede una risorsa protetta, ad esempio il file *Web. config* , o che si trova in una cartella protetta (ad esempio, *dati di app\_* o *codice di\_dell'app*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Questo tipo di pagina non viene servito (errore del server nell'applicazione '/')

Vedere la descrizione precedente per l'errore HTTP 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemi con il codice Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Il nome '*Class*' non esiste nel contesto corrente

Spesso, un motivo per cui viene visualizzato questo errore è che `class` fa riferimento a un helper, ma l'helper non è installato. Se, ad esempio, si tenta di usare un helper, ma se il pacchetto non è stato installato da NuGet, verrà visualizzato questo errore. Usare la raccolta in WebMatrix per trovare e installare l'helper.

Se l'helper è installato, ma la pagina non lo riconosce ancora, provare ad aggiungere un'istruzione `using` al codice. Nell'istruzione `using` fare riferimento allo spazio dei nomi che include l'helper. Ad esempio, gli helper di base inclusi nel pacchetto ASP.NET Web Helper si trovano nello spazio dei nomi `System.Web.Helpers`. Nella parte superiore della pagina in cui si vuole usare l'helper aggiungere questa riga:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemi relativi alla sicurezza e all'appartenenza

Se si utilizza il sistema di sicurezza (appartenenza) incorporato in Pagine Web ASP.NET (Razor), è possibile che si verifichino i seguenti problemi.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Per chiamare questo metodo, la proprietà "Membership. provider" deve essere un'istanza di "ExtendedMembershipProvider"

Questo errore può indicare che non è configurata alcuna classe `AspNetSqlMembershipProvider`. Un sintomo è che il sito funziona correttamente in locale, ma genera questo errore quando lo si pubblica nel server di un provider di hosting. Una correzione per questo problema consiste nell'abilitare esplicitamente l'appartenenza semplice aggiungendo quanto segue al file *Web. config* del sito:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemi relativi all'invio di posta elettronica

Problemi con l'invio di messaggi di posta elettronica possono essere difficili da eseguire il debug. Un problema iniziale può essere che non è possibile connettersi al server SMTP. Se la connessione ha esito positivo, ASP.NET invia il messaggio al server SMTP. Tuttavia, possono verificarsi problemi con il messaggio stesso che impedisce al server SMTP di inviarlo.

Se l'applicazione non invia messaggi di posta elettronica correttamente, provare a eseguire le operazioni seguenti:

- Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito in un provider di hosting, il nome del server SMTP in quel momento potrebbe essere `localhost`. Questa situazione si verifica perché dopo la pubblicazione e il sito è in esecuzione nel server del provider, il server SMTP potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi dei server potrebbe significare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.
- Il numero di porta è in genere 25. Tuttavia, per alcuni provider è necessario usare la porta 587 o un'altra porta. Verificare con il proprietario del server SMTP il numero di porta che si prevede di usare.
- Assicurarsi di usare le credenziali corrette. Se il sito è stato pubblicato in un provider di hosting, utilizzare le credenziali indicate in modo specifico dal provider per la posta elettronica. Queste credenziali potrebbero essere diverse dalle credenziali usate per la pubblicazione.
- In alcuni casi non è necessaria alcuna credenziale. Se si sta inviando un messaggio di posta elettronica tramite l'ISP personale, è possibile che il provider di posta elettronica conosca già le credenziali. Dopo la pubblicazione, potrebbe essere necessario utilizzare credenziali diverse rispetto a quando si esegue il test nel computer locale.
- Se il provider di posta elettronica utilizza la crittografia, impostare `WebMail.EnableSsl` su `true`.

Se si verifica un errore durante l'invio della posta elettronica, è possibile che venga visualizzato un messaggio di errore ASP.NET standard, simile al seguente:

![Messaggio di errore ASP.NET quando si verifica un problema con la posta elettronica](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

È anche possibile eseguire il debug di problemi con l'invio di messaggi di posta elettronica usando un blocco di `try-catch`, come nell'esempio seguente. Quando si usa un blocco di `try-catch`, ASP.NET non Visualizza i messaggi di errore standard. È invece possibile acquisire l'errore nella `catch` parte del blocco.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sostituire i valori appropriati per `your-SMTP-server-name`e così via. Di seguito sono riportati alcuni dei messaggi di errore che potrebbero essere visualizzati in questo modo:

- *Errore durante l'invio della posta.*

    -oppure-

    *Tentativo di connessione non riuscito perché l'entità connessa non ha risposto correttamente dopo un periodo di tempo o la connessione stabilita non è riuscita perché l'host connesso non ha risposto*

    Questo errore indica in genere che l'applicazione non è in grado di connettersi al server SMTP. Verificare il nome del server e il numero di porta.
- *Cassetta postale non disponibile. Risposta server: 5.1.0 &lt;someuser@invaliddomain&gt; mittente rifiutato: dominio mittente non valido*

    Questo messaggio può indicare che l'indirizzo del `From` non è corretto o è mancante.
- *Il formato della stringa specificata non è obbligatorio per un indirizzo di posta elettronica.*

    Questo errore può indicare che il valore delle proprietà `To` o `From` non è riconosciuto come indirizzo di posta elettronica. (ASP.NET non è in grado di verificare che l'indirizzo di posta elettronica sia valido, solo che sia nel formato corretto, ad esempio *name@domain.com* ).

> [!NOTE]
> Rimuovere il markup che Visualizza l'errore (`@errorMessage`) prima di pubblicare la pagina in un sito attivo. Non è consigliabile consentire agli utenti di visualizzare i messaggi di errore che si ottengono da un server.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Domande frequenti sulle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

Forum su [WebMatrix e pagine Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) nel sito Web ASP.NET
