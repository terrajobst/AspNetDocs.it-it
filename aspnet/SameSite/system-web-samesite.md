---
title: Usare i cookie navigava sullostesso sito in ASP.NET
author: rick-anderson
description: Informazioni su come usare per navigava sullostesso sito i cookie in ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c81ca38648609aa5347d2a8cc11889fc85d81711
ms.sourcegitcommit: 4d439e01c82c7c95b19216fedaf5b1a11a1deb06
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76826614"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Usare i cookie navigava sullostesso sito in ASP.NET

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Navigava sullostesso sito è uno standard [IETF](https://ietf.org/about/) Draft progettato per garantire una protezione contro gli attacchi di richiesta intersito falsificazione (CSRF). Originariamente redatto in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), il Draft standard è stato aggiornato in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). Lo standard aggiornato non è compatibile con le versioni precedenti dello standard precedente, con le differenze più evidenti tra quelle riportate di seguito:

* I cookie senza intestazione navigava sullostesso sito vengono considerati `SameSite=Lax` per impostazione predefinita.
* `SameSite=None` deve essere usato per consentire l'uso di cookie tra siti.
* I cookie che asseriscono `SameSite=None` devono essere anche contrassegnati come `Secure`.
* Il valore navigava sullostesso sito = None non è consentito dallo [standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) e fa in modo che alcune implementazioni considerino tali cookie come navigava sullostesso sito = Strict. Vedere [supporto di browser meno recenti](#sob) in questo documento.

L'impostazione `SameSite=Lax` funziona per la maggior parte dei cookie dell'applicazione. Alcune forme di autenticazione come [OpenID Connect](https://openid.net/connect/) (OIDC) e l'impostazione predefinita di [WS-Federation](https://auth0.com/docs/protocols/ws-fed) sono i reindirizzamenti basati su post. I reindirizzamenti basati su POST attivano le protezioni del browser navigava sullostesso sito, pertanto navigava sullostesso sito è disabilitato per questi componenti. La maggior parte degli accessi [OAuth](https://oauth.net/) non è interessata a causa di differenze nel modo in cui la richiesta fluisce.

Le applicazioni che usano `iframe` possono riscontrare problemi con `SameSite=Lax` o `SameSite=Strict` cookie perché gli iframe sono considerati scenari tra siti.

Ogni componente ASP.NET che emette cookie deve decidere se navigava sullostesso sito è appropriato.

Vedere [problemi noti](#known) relativi a problemi con le applicazioni dopo l'installazione degli aggiornamenti di navigava sullostesso sito .NET 2019.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Uso di navigava sullostesso sito in ASP.NET 4.7.2 e 4,8

.NET 4.7.2 e 4,8 supporta lo [standard 2019 Draft](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) per navigava sullostesso sito dal rilascio degli aggiornamenti nel 2019 dicembre. Gli sviluppatori sono in grado di controllare a livello di codice il valore dell'intestazione navigava sullostesso sito usando la [Proprietà HttpCookie. navigava sullostesso sito](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite). Impostando la proprietà `SameSite` su `Strict`, `Lax`o `None` i valori vengono scritti in rete con il cookie. Se è uguale a `(SameSiteMode)(-1)` indica che non è necessario includere nella rete alcuna intestazione navigava sullostesso sito con il cookie. È possibile usare la [Proprietà HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure)o ' RequireSSL ' nei file di configurazione per contrassegnare il cookie come `Secure` o meno.

Per impostazione predefinita, le nuove istanze di `HttpCookie` vengono `SameSite=(SameSiteMode)(-1)` e `Secure=false`. Queste impostazioni predefinite possono essere sostituite nella sezione `system.web/httpCookies` Configuration, in cui la stringa `"Unspecified"` è una sintassi intuitiva di sola configurazione per `(SameSiteMode)(-1)`:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

ASP.Net emette anche quattro cookie specifici per queste funzionalità: autenticazione anonima, autenticazione basata su form, stato sessione e gestione dei ruoli. Le istanze di questi cookie ottenute in fase di esecuzione possono essere modificate usando le proprietà `SameSite` e `Secure` esattamente come qualsiasi altra istanza di HttpCookie. Tuttavia, a causa dell'emergenza patchwork dello standard navigava sullostesso sito, le opzioni di configurazione per questi quattro cookie di funzionalità sono incoerenti. Di seguito sono riportati gli attributi e le sezioni di configurazione pertinenti, con le impostazioni predefinite. Se non è presente alcun `SameSite` o `Secure` attributo correlato per una funzionalità, la funzionalità eseguirà il fallback sui valori predefiniti configurati nella sezione `system.web/httpCookies` descritta in precedenza.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequiresSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Nota**:' Unspecified ' è disponibile solo per `system.web/httpCookies@sameSite` al momento. Ci auguriamo di aggiungere una sintassi simile agli attributi cookieSameSite illustrati in precedenza negli aggiornamenti futuri. L'impostazione di `(SameSiteMode)(-1)` nel codice funziona comunque con le istanze di questi cookie. *

## <a name="history-and-changes"></a>Cronologia e modifiche

Il supporto di navigava sullostesso sito è stato implementato per la prima volta in .NET 4.7.2 usando lo [standard draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Il 19 novembre 2019 aggiornamenti per Windows aggiornamento di .NET 4.7.2 + dallo standard 2016 allo standard 2019. Ulteriori aggiornamenti sono imminenti per le altre versioni di Windows. Per ulteriori informazioni, vedere <xref:samesite/kbs-samesite>.

 Bozza 2019 della specifica navigava sullostesso sito:

* **Non** è compatibile con le versioni precedenti della bozza 2016. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.
* Specifica che i cookie vengono considerati come `SameSite=Lax` per impostazione predefinita.
* Specifica i cookie che asseriscono in modo esplicito `SameSite=None` per consentire il recapito tra siti devono essere anche contrassegnati come `Secure`.
* È supportato dalle patch rilasciate come descritto nella Knowledge base elencata in precedenza.
* Viene pianificata per essere abilitata per impostazione predefinita da [Chrome](https://chromestatus.com/feature/5088147346030592) in [feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Il passaggio a questo standard nei browser è stato avviato in 2019.

<a name="known"><a/>

## <a name="known-issues"></a>Problemi noti

Poiché le specifiche draft 2016 e 2019 non sono compatibili, l'aggiornamento di novembre 2019 .NET Framework introduce alcune modifiche che potrebbero compromettere.

* Lo stato della sessione e i cookie di autenticazione basata su form vengono ora scritti nella rete come `Lax` anziché non specificati.
  * Sebbene la maggior parte delle app funzioni con `SameSite=Lax` cookie, le app che PUBBLICAno i siti o le applicazioni che usano `iframe` potrebbero scoprire che i cookie di stato della sessione o di autorizzazione basata su form non vengono usati come previsto. Per risolvere questo problema, modificare il valore di `cookieSameSite` nella sezione di configurazione appropriata come descritto in precedenza.
* HttpCookies che impostano in modo esplicito `SameSite=None` nel codice o nella configurazione ora hanno tale valore scritto con il cookie, mentre in precedenza è stato omesso. Ciò può causare problemi con i browser meno recenti che supportano solo lo standard 2016 Draft.
  * Quando la destinazione è il browser che supporta lo standard 2019 Draft con `SameSite=None` cookie, ricordarsi di contrassegnarli anche `Secure` o non riconosciuti.
  * Per ripristinare il comportamento 2016 di non scrivere `SameSite=None`, usare l'impostazione app `aspnet:SupressSameSiteNone=true`. Si noti che questa operazione verrà applicata a tutti HttpCookies nell'app.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Servizio app Azure-gestione dei cookie navigava sullostesso sito

Per informazioni sul modo in cui app Azure servizio sta configurando i comportamenti navigava sullostesso sito nelle app .NET 4.7.2 [, vedere app Azure Service-gestione dei cookie navigava sullostesso sito e .NET Framework patch 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Supporto di browser meno recenti

Lo standard 2016 navigava sullostesso sito ha richiesto che i valori sconosciuti debbano essere considerati come valori `SameSite=Strict`. Le app a cui è stato eseguito l'accesso da browser meno recenti che supportano lo standard navigava sullostesso sito 2016 possono interrompersi quando ottengono una proprietà navigava sullostesso sito con un valore di `None`. Le app Web devono implementare il rilevamento del browser se intendono supportare browser meno recenti. ASP.NET non implementa il rilevamento del browser perché i valori degli agenti utente sono altamente volatili e cambiano di frequente. Il codice seguente può essere chiamato nel <xref:HTTP.HttpCookie> sito di chiamata:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Nell'esempio precedente `MyUserAgentDetectionLib.DisallowsSameSiteNone` è una libreria fornita dall'utente che rileva se l'agente utente non supporta il `None`navigava sullostesso sito. Il codice seguente illustra un metodo di `DisallowsSameSiteNone` di esempio:

> [!WARNING]
> Il codice seguente è solo a scopo dimostrativo:
> * Non deve essere considerato completo.
> * Non è gestita né supportata.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testare le app per i problemi di navigava sullostesso sito

Le app che interagiscono con i siti remoti, ad esempio tramite l'accesso di terze parti, devono:

* Testare l'interazione su più browser.
* Applicare il [rilevamento e la mitigazione del browser](#sob) discussi in questo documento.

Testare le app Web usando una versione client che può acconsentire esplicitamente al nuovo comportamento del navigava sullostesso sito. Chrome, Firefox e Chromium Edge hanno tutti nuovi flag di funzionalità di consenso esplicito che possono essere usati per i test. Quando l'app applica le patch di navigava sullostesso sito, testarla con versioni client precedenti, in particolare Safari. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.

### <a name="test-with-chrome"></a>Eseguire test con Chrome

Chrome 78 + fornisce risultati fuorvianti perché prevede una mitigazione temporanea. La mitigazione temporanea di Chrome 78 + consente ai cookie meno di due minuti di età. Chrome 76 o 77 con i flag di test appropriati abilitati fornisce risultati più accurati. Per testare il nuovo comportamento di **navigava sullostesso sito, attivare**o disabilitare `chrome://flags/#same-site-by-default-cookies`. Le versioni precedenti di Chrome (75 e versioni precedenti) vengono segnalate per avere esito negativo con la nuova impostazione `None`. Vedere [supporto di browser meno recenti](#sob) in questo documento.

Google non rende disponibili versioni precedenti di Chrome. Seguire le istruzioni riportate in [scaricare Chromium](https://www.chromium.org/getting-involved/download-chromium) per testare le versioni precedenti di Chrome. Non **scaricare Chrome** dai collegamenti forniti cercando le versioni precedenti di Chrome.

* [Cromo 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Cromo 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Eseguire test con Safari

Safari 12 ha implementato rigorosamente la bozza precedente e ha esito negativo quando il nuovo valore `None` si trova in un cookie. `None` viene evitata tramite il codice di rilevamento del browser che [supporta i browser meno recenti](#sob) in questo documento. Testare gli accessi basati su stile sistema operativo Safari 12, Safari 13 e WebKit usando MSAL, ADAL o qualsiasi libreria in uso. Il problema dipende dalla versione del sistema operativo sottostante. I problemi di compatibilità con il nuovo comportamento navigava sullostesso sito sono noti per OSX Mojave (10,14) e iOS 12. L'aggiornamento del sistema operativo a OSX Catalina (10,15) o iOS 13 corregge il problema. Safari attualmente non dispone di un flag di consenso esplicito per il test del nuovo comportamento delle specifiche.

### <a name="test-with-firefox"></a>Eseguire test con Firefox

Il supporto di Firefox per il nuovo standard può essere testato nella versione 68 + scegliendo nella pagina `about:config` con il flag funzionalità `network.cookie.sameSite.laxByDefault`. Non sono stati segnalati problemi di compatibilità con le versioni precedenti di Firefox.

### <a name="test-with-edge-browser"></a>Eseguire test con il browser Microsoft Edge

Microsoft Edge supporta lo standard navigava sullostesso sito precedente. La versione perimetrale 44 non presenta problemi di compatibilità noti con il nuovo standard.

### <a name="test-with-edge-chromium"></a>Test con Microsoft Edge (Chromium)

I flag navigava sullostesso sito sono impostati nella pagina `edge://flags/#same-site-by-default-cookies`. Nessun problema di compatibilità rilevato con cromo perimetrale.

### <a name="test-with-electron"></a>Test con elettrone

Le versioni di Electron includono versioni precedenti di cromo. Ad esempio, la versione di Electron usata dai team è Chromium 66, che presenta il comportamento precedente. È necessario eseguire test di compatibilità personalizzati con la versione di Electron utilizzata dal prodotto. Vedere [supporto dei browser precedenti](#sob) nella sezione seguente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Prossime modifiche ai cookie navigava sullostesso sito in ASP.NET e ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog di Chromium: sviluppatori: prepararsi per la nuova navigava sullostesso sito = None; Impostazioni sicure del cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Spiegazione dei cookie navigava sullostesso sito](https://web.dev/samesite-cookies-explained/)
