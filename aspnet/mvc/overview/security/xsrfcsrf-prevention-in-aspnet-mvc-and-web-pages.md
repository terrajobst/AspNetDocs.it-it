---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenzione di XSRF/CSRF in ASP.NET MVC e Web Pages | Microsoft Docs
author: Rick-Anderson
description: La richiesta tra siti falsificata (nota anche come XSRF o CSRF) è un attacco contro le applicazioni ospitate sul Web, in cui un sito Web dannoso può influenzare l'interazione...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: fb7e76101cbe6a874ddf5b3429ca2dc6d474334b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595756"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenzione di XSRF/CSRF in ASP.NET MVC e pagine Web

di [Rick Anderson]((https://twitter.com/RickAndMSFT))

> La richiesta tra siti falsificata (nota anche come XSRF o CSRF) è un attacco contro le applicazioni ospitate sul Web, in cui un sito Web dannoso può influenzare l'interazione tra un browser client e un sito web considerato attendibile da tale browser. Questi attacchi sono possibili perché i Web browser invieranno automaticamente i token di autenticazione con ogni richiesta a un sito Web. L'esempio canonico è un cookie di autenticazione, ad esempio ASP. Ticket di autenticazione basata su form di NET. Tuttavia, i siti Web che utilizzano qualsiasi meccanismo di autenticazione permanente, ad esempio l'autenticazione di Windows, di base e così via, possono essere assegnati a tali attacchi.
> 
> Un attacco XSRF è diverso da un attacco di phishing. Gli attacchi di phishing richiedono interazione da parte della vittima. In un attacco di phishing, un sito Web dannoso simula il sito Web di destinazione e la vittima è ingannata a fornire informazioni riservate all'autore dell'attacco. In un attacco XSRF, spesso non è necessaria alcuna interazione da parte della vittima. Piuttosto, l'autore dell'attacco si basa sul browser che invia automaticamente tutti i cookie pertinenti al sito Web di destinazione.
> 
> Per ulteriori informazioni, vedere [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomia di un attacco

Per esaminare un attacco XSRF, considerare un utente che desidera eseguire alcune transazioni online bancarie. Questo utente visita prima WoodgroveBank.com e accede, a quel punto l'intestazione della risposta conterrà il cookie di autenticazione:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Poiché il cookie di autenticazione è un cookie di sessione, verrà automaticamente cancellato dal browser quando il processo del browser viene chiuso. Tuttavia, fino a quel momento, il browser includerà automaticamente il cookie con ogni richiesta a WoodgroveBank.com. L'utente ora desidera trasferire $1000 a un altro account, quindi compila un modulo nel sito bancario e il browser esegue questa richiesta al server:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Poiché questa operazione ha un effetto collaterale (avvia una transazione monetaria), il sito bancario ha scelto di richiedere un HTTP POST per poter avviare questa operazione. Il server legge il token di autenticazione dalla richiesta, Cerca il numero di account dell'utente corrente, verifica che esistano fondi sufficienti, quindi avvia la transazione nell'account di destinazione.

Il tuo online banking è stato completato, l'utente si sposta dal sito bancario e visita altre località sul Web. Uno di questi siti, fabrikam.com, include il markup seguente in una pagina incorporata all'interno di un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Che quindi fa in modo che il browser effettui questa richiesta:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

L'autore dell'attacco sta sfruttando il fatto che l'utente potrebbe ancora avere un token di autenticazione valido per il sito Web di destinazione e usa un piccolo frammento di codice JavaScript per fare in modo che il browser renda automaticamente un HTTP POST al sito di destinazione. Se il token di autenticazione è ancora valido, il sito bancario avvierà un trasferimento di $250 nell'account dell'utente malintenzionato scegliendo.

### <a name="ineffective-mitigations"></a>Mitigazioni inefficaci

È interessante notare che nello scenario precedente il fatto che WoodgroveBank.com è stato eseguito l'accesso tramite SSL e che un cookie di autenticazione solo SSL non era sufficiente per contrastare l'attacco. L'utente malintenzionato è in grado di specificare lo [schema URI](http://en.wikipedia.org/wiki/URI_scheme) (HTTPS) nell'elemento &lt;form&gt; e il browser continuerà a inviare cookie non scaduti al sito di destinazione, purché tali cookie siano coerenti con lo schema URI della destinazione prevista.

Si può affermare che l'utente non deve semplicemente visitare i siti non attendibili, perché la visita solo dei siti attendibili consente di mantenere la sicurezza online. Esistono alcune verità, ma sfortunatamente questo Consiglio non è sempre pratico. Probabilmente l'utente "considera attendibile" il sito delle notizie locali ConsolidatedMessenger. ConsolidatedMessenger.com e visita invece il sito, ma il sito ha una vulnerabilità XSS che consente a un utente malintenzionato di inserire lo stesso frammento di codice in esecuzione in fabrikam.com.

È possibile verificare che le richieste in ingresso dispongano di un' [intestazione Referer che fa](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) riferimento al dominio. Questa operazione arresterà le richieste involontariamente inviate da un dominio di terze parti. Tuttavia, alcune persone disabilitano l'intestazione del referer del browser per motivi di privacy e talvolta gli utenti malintenzionati possono falsificare tale intestazione se la vittima dispone di un software non sicuro installato. La verifica dell' [intestazione del referer](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) non è considerata un approccio sicuro per impedire attacchi XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Mitigazioni XSRF di runtime dello stack Web

Il runtime di ASP.NET Web stack usa una variante del [modello di token di sincronizzazione](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) per difendersi dagli attacchi XSRF. Il formato generale del modello di token di sincronizzazione è che due token anti-XSRF vengono inviati al server con ogni HTTP POST (oltre al token di autenticazione): un token come cookie e l'altro come valore del modulo. I valori dei token generati dal runtime ASP.NET non sono deterministici o prevedibili da un utente malintenzionato. Quando vengono inviati i token, il server consentirà alla richiesta di procedere solo se entrambi i token superano un controllo di confronto.

Il *token della sessione* di verifica della richiesta XSRF viene archiviato come cookie HTTP e attualmente contiene le informazioni seguenti nel payload:

- Token di sicurezza costituito da un identificatore casuale a 128 bit.   
 La figura seguente mostra il token della sessione di verifica della richiesta XSRF visualizzato con gli strumenti di sviluppo F12 di Internet Explorer: (si noti che questa è l'implementazione corrente ed è soggetta, anche probabilmente, a modificare).

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Il *token del campo* viene archiviato come `<input type="hidden" />` e contiene le informazioni seguenti nel payload:

- Nome utente dell'utente connesso (se autenticato).
- Eventuali dati aggiuntivi forniti da un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

I payload dei token anti-XSRF sono crittografati e firmati, pertanto non è possibile visualizzare il nome utente quando si usano gli strumenti per esaminare i token. Quando l'applicazione Web è destinata a ASP.NET 4,0, i servizi di crittografia sono forniti dalla routine [machineKey. Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) . Quando l'applicazione Web è destinata a ASP.NET 4,5 o versione successiva, i servizi di crittografia sono forniti dalla routine [machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) , che offre prestazioni migliori, estendibilità e sicurezza. Per altri dettagli, vedere i post di Blog seguenti:

- [Miglioramenti crittografici in ASP.NET 4,5, PT. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Miglioramenti crittografici in ASP.NET 4,5, PT. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Miglioramenti crittografici in ASP.NET 4,5, PT. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generazione dei token

Per generare i token anti-XSRF, chiamare il metodo [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) da una visualizzazione MVC o @AntiForgery.GetHtml() da una pagina Razor. Il runtime eseguirà quindi i passaggi seguenti:

1. Se la richiesta HTTP corrente contiene già un token di sessione anti-XSRF (il cookie anti-XSRF \_\_RequestVerificationToken), il token di sicurezza viene Estratto da tale token. Se la richiesta HTTP non contiene un token di sessione anti-XSRF o se l'estrazione del token di sicurezza ha esito negativo, verrà generato un nuovo token anti-XSRF casuale.
2. Un token di campo anti-XSRF viene generato utilizzando il token di sicurezza del passaggio (1) precedente e l'identità dell'utente che ha eseguito l'accesso corrente. Per ulteriori informazioni sulla determinazione dell'identità utente, vedere la sezione **[scenari con supporto speciale](#_Scenarios_with_special)** di seguito. Inoltre, se è configurato un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) , il runtime chiamerà il relativo metodo [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) e includerà la stringa restituita nel token del campo. Per ulteriori informazioni, vedere la sezione relativa alla **[configurazione e all'estendibilità](#_Configuration_and_extensibility)** .
3. Se è stato generato un nuovo token anti-XSRF nel passaggio (1), verrà creato un nuovo token di sessione che lo conterrà e verrà aggiunto alla raccolta di cookie HTTP in uscita. Il token di campo del passaggio (2) verrà racchiuso in un elemento `<input type="hidden" />` e il markup HTML sarà il valore restituito di `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Convalida dei token

Per convalidare i token anti-XSRF in ingresso, lo sviluppatore include un attributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) sul suo controller o azione MVC oppure chiama `@AntiForgery.Validate()` dalla propria pagina Razor. Il runtime eseguirà i passaggi seguenti:

1. Il token di sessione in ingresso e il token di campo vengono letti e il token anti-XSRF Estratto da ciascuno. I token anti-XSRF devono essere identici per ogni passaggio (2) nella routine di generazione.
2. Se l'utente corrente è autenticato, il suo nome utente viene confrontato con il nome utente archiviato nel token del campo. I nomi utente devono corrispondere.
3. Se è configurato un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , il runtime chiama il relativo metodo *ValidateAdditionalData* . Il metodo deve restituire il valore booleano *true*.

Se la convalida ha esito positivo, la richiesta può continuare. Se la convalida ha esito negativo, il Framework genererà un *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condizioni di errore

A partire da ASP.NET Web stack Runtime V2, qualsiasi *HttpAntiForgeryException* generato durante la convalida conterrà informazioni dettagliate su ciò che si è verificato un errore. Le condizioni di errore attualmente definite sono:

- Il token di sessione o il token del form non è presente nella richiesta.
- Il token di sessione o il token del modulo non è leggibile. La causa più probabile è una farm che esegue versioni non corrispondenti del runtime dello stack Web ASP.NET o di una farm in cui l'elemento&gt; &lt;machineKey in Web. config è diverso tra i computer. È possibile utilizzare uno strumento come Fiddler per forzare questa eccezione manomettendo il token anti-XSRF.
- Il token di sessione e il token di campo sono stati scambiati.
- Il token di sessione e il token di campo contengono token di sicurezza non corrispondenti.
- Il nome utente incorporato nel token del campo non corrisponde al nome utente dell'utente connesso corrente.
- Il metodo *[IAntiForgeryAdditionalDataProvider. ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* ha restituito *false*.

Le funzionalità anti-XSRF possono anche eseguire ulteriori verifiche durante la generazione o la convalida dei token e gli errori durante tali controlli possono causare la generazione di eccezioni. Per ulteriori informazioni, vedere le sezioni [WIF/ACS/autenticazione basata su attestazioni](#_WIF_ACS) e **[estendibilità ed estendibilità](#_Configuration_and_extensibility)** .

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Scenari con supporto speciale

### <a name="anonymous-authentication"></a>Autenticazione anonima

Il sistema XSRF contiene un supporto speciale per gli utenti anonimi, in cui "Anonymous" viene definito come un utente in cui la proprietà *IIdentity. IsAuthenticated* restituisce *false*. Gli scenari includono la protezione XSRF per la pagina di accesso (prima dell'autenticazione dell'utente) e gli schemi di autenticazione personalizzati in cui l'applicazione usa un meccanismo diverso da *IIdentity* per identificare gli utenti.

Per supportare questi scenari, tenere presente che i token di sessione e di campo sono uniti da un token di sicurezza, ovvero un identificatore opaco a 128 bit generato in modo casuale. Questo token di sicurezza viene usato per tenere traccia di una sessione di un singolo utente mentre Esplora il sito, in modo che sia in grado di svolgere efficacemente lo scopo di un identificatore anonimo. Viene utilizzata una stringa vuota al posto del nome utente per le routine di generazione e convalida descritte in precedenza.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/autenticazione basata su attestazioni

In genere, le classi *IIdentity* incorporate nel .NET Framework hanno la proprietà che *IIdentity.Name* è sufficiente per identificare in modo univoco un determinato utente all'interno di una particolare applicazione. Ad esempio, *FormsIdentity.Name* restituisce il nome utente archiviato nel database delle appartenenze (univoco per tutte le applicazioni a seconda del database), *WindowsIdentity.Name* restituisce l'identità dell'utente qualificata come dominio e così via. Questi sistemi forniscono non solo l'autenticazione; *identificano* inoltre gli utenti di un'applicazione.

L'autenticazione basata sulle attestazioni, d'altra parte, non richiede necessariamente l'identificazione di un utente specifico. Al contrario, i tipi *ClaimsPrincipal* e *ClaimsIdentity* sono associati a un set di istanze di *attestazione* , dove le singole attestazioni potrebbero essere "è di 18 + anni di età" o "è un amministratore" per qualsiasi altro elemento. Poiché l'utente non è stato necessariamente identificato, il runtime non può utilizzare la proprietà *ClaimsIdentity.Name* come identificatore univoco per questo particolare utente. Il team ha visto esempi reali in cui *ClaimsIdentity.Name* restituisce *null*, restituisce un nome descrittivo (visualizzazione) o restituisce una stringa che non è appropriata per l'uso come identificatore univoco per l'utente.

Molte distribuzioni che usano l'autenticazione basata sulle attestazioni usano il [servizio di controllo di accesso di Azure](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) in particolare. ACS consente allo sviluppatore di configurare i singoli *provider di identità* , ad esempio ADFS, il provider di account Microsoft, i provider OpenID come Yahoo! e così via, e i provider di identità restituiscono *identificatori di nome*. Questi identificatori di nome possono contenere informazioni personali, ad esempio un indirizzo di posta elettronica, oppure possono essere resi anonimi come un identificatore personale privato (PPID). Indipendentemente dalla tupla, ovvero il provider di identità, l'identificatore del nome, funge sufficientemente da token di rilevamento appropriato per un determinato utente mentre Esplora il sito, in modo che il runtime di ASP.NET Web stack possa usare la tupla al posto del nome utente durante la generazione di e convalida di token di campo anti-XSRF. Gli URI specifici per il provider di identità e l'identificatore del nome sono:

- `https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(per altre informazioni, vedere la [pagina del documento ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) ).

Quando si genera o convalida un token, il runtime dello stack Web ASP.NET in fase di esecuzione tenterà di eseguire l'associazione ai tipi:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (per WIF SDK).
- `System.Security.Claims.ClaimsIdentity` (per .NET 4,5).

Se questi tipi esistono e se il *IIIIdentity* dell'utente corrente implementa o sottoclassa uno di questi tipi, la funzionalità anti-XSRF utilizzerà la tupla (provider di identità, identificatore nome) al posto del nome utente durante la generazione e la convalida dei token. Se non è presente una tupla di questo tipo, la richiesta avrà esito negativo e verrà visualizzato un errore che descrive allo sviluppatore come configurare il sistema anti-XSRF per comprendere il meccanismo di autenticazione basato sulle attestazioni specifico in uso. Per ulteriori informazioni, vedere la sezione relativa alla **[configurazione e all'estendibilità](#_Configuration_and_extensibility)** .

### <a name="oauth--openid-authentication"></a>Autenticazione OAuth/OpenID

Infine, la funzionalità anti-XSRF ha un supporto speciale per le applicazioni che usano l'autenticazione OAuth o OpenID. Questo supporto è basato su euristica: se il *IIdentity.Name* corrente inizia con http://o https://, i confronti del nome utente verranno eseguiti usando un operatore di confronto ordinale anziché l'operatore di confronto OrdinalIgnoreCase predefinito.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configurazione ed estendibilità

Occasionalmente, gli sviluppatori possono avere un controllo più rigoroso sui comportamenti di generazione e convalida anti-XSRF. Ad esempio, forse il comportamento predefinito degli helper di MVC e pagine Web per l'aggiunta automatica di cookie HTTP alla risposta è indesiderato e lo sviluppatore potrebbe desiderare di salvare in modo permanente i token altrove. Sono disponibili due API per semplificare questa operazione:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Il metodo *GetTokens* accetta come input un token della sessione di verifica della richiesta XSRF esistente (che potrebbe essere null) e produce come output un nuovo token di sessione e un token di sessione di verifica della richiesta XSRF. I token sono semplicemente stringhe opache senza decorazione; il valore *formToken* non verrà incluso nel tag di input&gt; di &lt;. Il valore di *newCookieToken* può essere null. in tal caso, il valore *oldCookieToken* è ancora valido e non è necessario impostare alcun nuovo cookie di risposta. Il chiamante di *GetTokens* è responsabile della permanenza dei cookie di risposta necessari o della generazione di eventuali markup necessari; il metodo *GetTokens* non modifica la risposta come effetto collaterale. Il metodo *Validate* accetta i token di sessione e di campo in ingresso e ne esegue la logica di convalida indicata sopra.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Lo sviluppatore può configurare il sistema anti-XSRF dall'avvio dell'applicazione\_. La configurazione è a livello di codice. Le proprietà del tipo *AntiForgeryConfig* statico sono descritte di seguito. Per la maggior parte degli utenti che usano le attestazioni è necessario impostare la proprietà UniqueClaimTypeIdentifier.

| **Property** | **Descrizione** |
| --- | --- |
| **AdditionalDataProvider** | Un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) che fornisce dati aggiuntivi durante la generazione di token e utilizza dati aggiuntivi durante la convalida del token. Il valore predefinito è *null*. Per ulteriori informazioni, vedere la sezione [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **CookieName** | Stringa che fornisce il nome del cookie HTTP usato per archiviare il token di sessione anti-XSRF. Se questo valore non è impostato, verrà generato automaticamente un nome in base al percorso virtuale distribuito dell'applicazione. Il valore predefinito è *null*. |
| **RequireSsl** | Valore booleano che determina se è necessario inviare i token anti-XSRF su un canale protetto da SSL. Se questo valore è *true*, tutti i cookie generati automaticamente avranno il flag "Secure" impostato e le API anti-XSRF generano se vengono chiamate dall'interno di una richiesta non inviata tramite SSL. Il valore predefinito è *false*. |
| **SuppressIdentityHeuristicChecks** | Valore booleano che determina se il sistema anti-XSRF deve disattivare il supporto per le identità basate sulle attestazioni. Se questo valore è *true*, il sistema presuppone che *IIdentity.Name* sia appropriato per l'uso come identificatore univoco per singolo utente e non tenterà di *IClaimsIdentity* o *CLCLAIMSIDENTITY* speciale come descritto in [WIF/ACS/ sezione autenticazione basata sulle attestazioni](#_WIF_ACS) . Il valore predefinito è `false`. |
| **UniqueClaimTypeIdentifier** | Stringa che indica il tipo di attestazione appropriato per l'utilizzo come identificatore univoco per utente. Se questo valore è impostato e l'oggetto *IIdentity* corrente è basato sulle attestazioni, il sistema tenterà di estrarre un'attestazione del tipo specificato da *UniqueClaimTypeIdentifier*e verrà usato il valore corrispondente al posto del nome utente quando generazione del token di campo. Se il tipo di attestazione non viene trovato, il sistema non riuscirà a eseguire la richiesta. Il valore predefinito è *null*, che indica che il sistema deve usare la tupla (provider di identità, identificatore nome) come descritto in precedenza al posto del nome utente dell'utente. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Il tipo *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* consente agli sviluppatori di estendere il comportamento del sistema anti-XSRF eseguendo il round trip di dati aggiuntivi in ogni token. Il metodo *GetAdditionalData* viene chiamato ogni volta che viene generato un token di campo e il valore restituito viene incorporato all'interno del token generato. Un responsabile dell'implementazione può restituire un timestamp, un parametro nonce o qualsiasi altro valore che desidera da questo metodo.

Analogamente, il metodo *ValidateAdditionalData* viene chiamato ogni volta che viene convalidato un token di campo e la stringa "dati aggiuntivi" incorporata all'interno del token viene passata al metodo. La routine di convalida può implementare un timeout (controllando l'ora corrente rispetto al tempo che è stato archiviato al momento della creazione del token), una routine di controllo nonce o qualsiasi altra logica desiderata.

## <a name="design-decisions-and-security-considerations"></a>Decisioni di progettazione e considerazioni sulla sicurezza

Il token di sicurezza che collega i token di sessione e campo è tecnicamente necessario solo quando si tenta di proteggere gli utenti anonimi o non autenticati da attacchi XSRF. Quando l'utente viene autenticato, il token di autenticazione (presumibilmente inviato sotto forma di cookie) può essere usato come metà di una coppia di token di sincronizzazione. Tuttavia, esistono scenari validi per la protezione delle pagine di accesso eseguite da utenti non autenticati e la logica anti-XSRF è stata semplificata generando sempre e convalidando il token di sicurezza, anche per gli utenti autenticati. Inoltre, fornisce una protezione aggiuntiva nel caso in cui un token di campo venga mai compromesso da un utente malintenzionato, perché l'impostazione o l'ipotesi di un token di sessione potrebbe essere un altro ostacolo per il superamento dell'autore dell'attacco.

Gli sviluppatori devono prestare attenzione quando più applicazioni sono ospitate in un singolo dominio. Ad esempio, anche se *Example1.cloudapp.NET* e *example2.cloudapp.NET* sono host diversi, esiste una relazione di trust implicita tra tutti gli host nel dominio *\*. cloudapp.NET* . Questa relazione di trust implicita [consente a host potenzialmente non attendibili di influenzare gli altri cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (i criteri di origine identici che regolano le richieste AJAX non si applicano necessariamente ai cookie HTTP). Il runtime di ASP.NET Web stack fornisce alcune attenuazioni nel fatto che il nome utente viene incorporato nel token di campo, quindi anche se un sottodominio dannoso è in grado di sovrascrivere un token di sessione, non sarà in grado di generare un token di campo valido per l'utente. Tuttavia, quando sono ospitati in un ambiente di questo tipo, le routine XSRF predefinite non possono ancora difendersi dal Hijack della sessione o dall'accesso XSRF.

Le routine anti-XSRF attualmente non difendono i [clickjacking](https://www.owasp.org/index.php/Clickjacking). Le applicazioni che desiderano difendersi da clickjacking possono eseguire questa operazione inviando un'intestazione X-frame-options: SAMEORIGIN a ogni risposta. Questa intestazione è supportata da tutti i browser recenti. Per ulteriori informazioni, vedere il [Blog di IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), il [Blog SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)e [OWASP](https://www.owasp.org/index.php/Clickjacking). Il runtime di ASP.NET Web stack potrebbe in alcune versioni future fare in modo che gli helper XSRF di MVC e le pagine Web consentano di impostare automaticamente questa intestazione in modo che le applicazioni vengano protette automaticamente da questo attacco.

Gli sviluppatori Web devono continuare a garantire che il sito non sia vulnerabile agli attacchi XSS. Gli attacchi XSS sono molto potenti e un exploit efficace comporta anche la violazione delle difese di runtime di ASP.NET Web stack contro gli attacchi XSRF.

## <a name="acknowledgment"></a>Riconoscimento

[@LeviBroderick](https://twitter.com/LeviBroderick), che ha scritto gran parte del codice di sicurezza di ASP.NET, la maggior parte di queste informazioni.
