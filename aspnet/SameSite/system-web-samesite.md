---
title: Usare i cookie navigava sullostesso sito in ASP.NET
author: rick-anderson
description: Informazioni su come usare per navigava sullostesso sito i cookie in ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 7987a5d6c9b3a82679d42a2d381d471d56f495c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546746"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Usare i cookie navigava sullostesso sito in ASP.NET

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Navigava sullostesso sito è uno standard [IETF](https://ietf.org/about/) Draft progettato per garantire una protezione contro gli attacchi di richiesta intersito falsificazione (CSRF). Originariamente redatto in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), il Draft standard è stato aggiornato in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). Lo standard aggiornato non è compatibile con le versioni precedenti dello standard precedente, con le differenze più evidenti tra quelle riportate di seguito:

* I cookie senza intestazione navigava sullostesso sito vengono considerati `SameSite=Lax` per impostazione predefinita.
* `SameSite=None` deve essere usato per consentire l'uso di cookie tra siti.
* I cookie che asseriscono `SameSite=None` devono essere anche contrassegnati come `Secure`.
* Le applicazioni che utilizzano [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) possono riscontrare problemi con `sameSite=Lax` o `sameSite=Strict` cookie perché `<iframe>` vengono considerati scenari tra siti.
* Il valore `SameSite=None` non è consentito dallo [standard 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) e fa in modo che alcune implementazioni gestiscano tali cookie come `SameSite=Strict`. Vedere [supporto di browser meno recenti](#sob) in questo documento.

L'impostazione `SameSite=Lax` funziona per la maggior parte dei cookie dell'applicazione. Alcune forme di autenticazione come [OpenID Connect](https://openid.net/connect/) (OIDC) e l'impostazione predefinita di [WS-Federation](https://auth0.com/docs/protocols/ws-fed) sono i reindirizzamenti basati su post. I reindirizzamenti basati su POST attivano le protezioni del browser navigava sullostesso sito, pertanto navigava sullostesso sito è disabilitato per questi componenti. La maggior parte degli accessi [OAuth](https://oauth.net/) non è interessata a causa di differenze nel modo in cui la richiesta fluisce.

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
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Nota**:' Unspecified ' è disponibile solo per `system.web/httpCookies@sameSite` al momento. Ci auguriamo di aggiungere una sintassi simile agli attributi cookieSameSite illustrati in precedenza negli aggiornamenti futuri. L'impostazione di `(SameSiteMode)(-1)` nel codice funziona comunque con le istanze di questi cookie. *

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a>Ridestinare le app .NET

Per specificare come destinazione .NET 4.7.2 o versione successiva:

* Verificare che *Web. config* contenga quanto segue:  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  Per informazioni dettagliate, fare [riferimento alla guida alla migrazione di .NET](/dotnet/framework/migration-guide/) .

* Verificare che i pacchetti NuGet nel progetto siano destinati alla versione del Framework corretta. È possibile verificare la versione corretta del Framework esaminando il file *packages. config* , ad esempio:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  Nel file *packages. config* precedente, il pacchetto di `Microsoft.ApplicationInsights`:
    * È destinato a .NET 4.5.1.
    * È necessario che il relativo attributo `targetFramework` venga aggiornato per `net472` se esiste un pacchetto aggiornato per la destinazione del Framework.

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a>Versioni di .NET precedenti a 4.7.2

Microsoft non supporta le versioni di .NET, più 4.7.2 per la scrittura dell'attributo del cookie dello stesso sito. Non è stato trovato alcun modo affidabile per:

* Verificare che l'attributo sia scritto correttamente in base alla versione del browser.
* Intercettare e modificare i cookie di autenticazione e di sessione nelle versioni precedenti del Framework.

### <a name="december-patch-behavior-changes"></a>Modifiche al comportamento della patch di dicembre

La modifica del comportamento specifico per .NET Framework è il modo in cui la proprietà `SameSite` interpreta il valore `None`:

* Prima della patch, il valore `None` significava:
  * Non creare l'attributo.
* Dopo la patch:
  * Il valore `None`significa "creare l'attributo con un valore di `None`".
  * Un valore `SameSite` `(SameSiteMode)(-1)` causa la mancata generazione dell'attributo.

Il valore predefinito di navigava sullostesso sito per l'autenticazione basata su form e i cookie di stato della sessione è stato modificato da `None` a `Lax`.

### <a name="summary-of-change-impact-on-browsers"></a>Riepilogo dell'effetto delle modifiche nei browser

Se si installa la patch e si rilascia un cookie con `SameSite.None`, viene eseguita una delle due operazioni seguenti:
* Chrome V80 tratterà questo cookie in base alla nuova implementazione e non imporrà le stesse restrizioni del sito sul cookie.
* Tutti i browser che non sono stati aggiornati per supportare la nuova implementazione seguiranno l'implementazione precedente. L'implementazione precedente afferma:
  * Se viene visualizzato un valore che non si comprende, ignorarlo e passare a restrizioni dello stesso sito.

Quindi, l'app si interrompe in Chrome o si interrompe in molte altre posizioni.

## <a name="history-and-changes"></a>Cronologia e modifiche

Il supporto di navigava sullostesso sito è stato implementato per la prima volta in .NET 4.7.2 usando lo [standard draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Il 19 novembre 2019 aggiornamenti per Windows aggiornamento di .NET 4.7.2 + dallo standard 2016 allo standard 2019. Ulteriori aggiornamenti sono imminenti per le altre versioni di Windows. Per altre informazioni, vedere <xref:samesite/kbs-samesite>.

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

Lo standard 2016 navigava sullostesso sito ha richiesto che i valori sconosciuti debbano essere considerati come valori `SameSite=Strict`. Le app a cui è stato eseguito l'accesso da browser meno recenti che supportano lo standard navigava sullostesso sito 2016 possono interrompersi quando ottengono una proprietà navigava sullostesso sito con un valore di `None`. Le app Web devono implementare il rilevamento del browser se intendono supportare browser meno recenti. ASP.NET non implementa il rilevamento del browser perché i valori degli agenti utente sono altamente volatili e cambiano di frequente.

L'approccio di Microsoft alla risoluzione del problema è quello di semplificare l'implementazione dei componenti di rilevamento del browser per rimuovere l'attributo `sameSite=None` dai cookie se un browser non lo supporta. Il Consiglio di Google è stato quello di emettere cookie doppi, uno con il nuovo attributo e uno senza l'attributo. Si considerino tuttavia i suggerimenti di Google limitati. Alcuni browser, in particolare i browser per dispositivi mobili, hanno limiti molto limitati al numero di cookie di un sito oppure possono inviare un nome di dominio. Inviare più cookie, soprattutto cookie di grandi dimensioni come i cookie di autenticazione, può raggiungere il limite del browser per dispositivi mobili molto rapidamente, causando errori delle app difficili da diagnosticare e correggere. Inoltre, come Framework è disponibile un ampio ecosistema di codice di terze parti e componenti che potrebbero non essere aggiornati per l'utilizzo di un approccio con doppio cookie.

Il codice di rilevamento del browser usato nei progetti di esempio in [questo repository GitHub]() è contenuto in due file

* [C#SameSiteSupport.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [VB SameSiteSupport. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

Questi rilevamenti sono gli agenti browser più comuni osservati che supportano lo standard 2016 e per i quali è necessario rimuovere completamente l'attributo. Non è concepito come un'implementazione completa:

* L'app può visualizzare i browser non disponibili nei siti di test.
* È necessario essere pronti ad aggiungere i rilevamenti necessari per l'ambiente.

La modalità di collegamento del rilevamento varia a seconda della versione di .NET e del framework Web in uso. Il codice seguente può essere chiamato nel sito di chiamata [HttpCookie](/dotnet/api/system.web.httpcookie) :

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Vedere gli argomenti seguenti relativi ai cookie ASP.NET 4.7.2 navigava sullostesso sito:

* [C#MVC](xref:samesite/csMVC)
* [C#WebForms](xref:samesite/CSharpWebForms)
* [WebForm VB](xref:samesite/vbWF)
* [MVC VB](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a>Verifica del reindirizzamento del sito a HTTPS

Per ASP.NET 4. x, Web Form e MVC, la funzionalità di [riscrittura URL di IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) può essere usata per reindirizzare tutte le richieste a HTTPS. Nel codice XML seguente viene illustrata una regola di esempio:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Nelle installazioni locali di [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) è una funzionalità facoltativa che potrebbe richiedere l'installazione di.

## <a name="test-apps-for-samesite-problems"></a>Testare le app per i problemi di navigava sullostesso sito

È necessario testare l'app con i browser supportati ed esaminare gli scenari che coinvolgono i cookie. Gli scenari di cookie coinvolgono generalmente

* Form di accesso
* Meccanismi di accesso esterni, ad esempio Facebook, Azure AD, OAuth e OIDC
* Pagine che accettano richieste da altri siti
* Pagine nell'app progettate per essere incorporate in iframe

Verificare che i cookie vengano creati, salvati in modo permanente ed eliminati correttamente nell'app.

Le app che interagiscono con i siti remoti, ad esempio tramite l'accesso di terze parti, devono:

* Testare l'interazione su più browser.
* Applicare il [rilevamento e la mitigazione del browser](#sob) discussi in questo documento.

Testare le app Web usando una versione client che può acconsentire esplicitamente al nuovo comportamento del navigava sullostesso sito. Chrome, Firefox e Chromium Edge hanno tutti nuovi flag di funzionalità di consenso esplicito che possono essere usati per i test. Quando l'app applica le patch di navigava sullostesso sito, testarla con versioni client precedenti, in particolare Safari. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.

### <a name="test-with-chrome"></a>Eseguire test con Chrome

Chrome 78 + fornisce risultati fuorvianti perché prevede una mitigazione temporanea. La mitigazione temporanea di Chrome 78 + consente ai cookie meno di due minuti di età. Chrome 76 o 77 con i flag di test appropriati abilitati fornisce risultati più accurati. Per testare il nuovo comportamento di **navigava sullostesso sito, attivare**o disabilitare `chrome://flags/#same-site-by-default-cookies`. Le versioni precedenti di Chrome (75 e versioni precedenti) vengono segnalate per avere esito negativo con la nuova impostazione `None`. Vedere [supporto di browser meno recenti](#sob) in questo documento.

Google non rende disponibili versioni precedenti di Chrome. Seguire le istruzioni riportate in [scaricare Chromium](https://www.chromium.org/getting-involved/download-chromium) per testare le versioni precedenti di Chrome. Non **scaricare Chrome** dai collegamenti forniti cercando le versioni precedenti di Chrome.

* [Cromo 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Cromo 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* Se non si usa una versione a 64 bit di Windows, è possibile usare il [Visualizzatore OmahaProxy](https://omahaproxy.appspot.com/) per cercare il ramo cromo corrispondente a Chrome 74 (v 74.0.3729.108) usando le [istruzioni fornite da Chromium](https://www.chromium.org/getting-involved/download-chromium).

A partire dalla versione Canary `80.0.3975.0`, la mitigazione di LAX + POST temporanea può essere disabilitata a scopo di test usando il nuovo flag `--enable-features=SameSiteDefaultChecksMethodRigorously` per consentire il test di siti e servizi nello stato finale finale della funzionalità in cui è stata rimossa la mitigazione. Per ulteriori informazioni, vedere la pagina relativa agli [aggiornamenti di navigava sullostesso sito](https://www.chromium.org/updates/same-site) per progetti Chromium

#### <a name="test-with-chrome-80"></a>Test con Chrome 80 +

[Scaricare](https://www.google.com/chrome/) una versione di Chrome che supporta il nuovo attributo. Al momento della stesura di questo documento, la versione corrente è Chrome 80. Chrome 80 richiede che il flag `chrome://flags/#same-site-by-default-cookies` abilitato per usare il nuovo comportamento. È inoltre consigliabile abilitare (`chrome://flags/#cookies-without-same-site-must-be-secure`) per testare il comportamento imminente per i cookie per i quali non sono abilitati attributi navigava sullostesso sito. Chrome 80 si trova nella destinazione per fare in modo che il Commuter tratti i cookie senza l'attributo come `SameSite=Lax`, anche se con un periodo di tolleranza programmato per determinate richieste. Per disabilitare il periodo di tolleranza temporizzato, Chrome 80 può essere avviato con l'argomento della riga di comando seguente:

`--enable-features=SameSiteDefaultChecksMethodRigorously`

Chrome 80 contiene messaggi di avviso nella console del browser sugli attributi navigava sullostesso sito mancanti. Usare F12 per aprire la console del browser.

### <a name="test-with-safari"></a>Eseguire test con Safari

Safari 12 ha implementato rigorosamente la bozza precedente e ha esito negativo quando il nuovo valore `None` si trova in un cookie. `None` viene evitata tramite il codice di rilevamento del browser che [supporta i browser meno recenti](#sob) in questo documento. Testare gli accessi basati su stile sistema operativo Safari 12, Safari 13 e WebKit usando MSAL, ADAL o qualsiasi libreria in uso. Il problema dipende dalla versione del sistema operativo sottostante. I problemi di compatibilità con il nuovo comportamento navigava sullostesso sito sono noti per OSX Mojave (10,14) e iOS 12. L'aggiornamento del sistema operativo a OSX Catalina (10,15) o iOS 13 corregge il problema. Safari attualmente non dispone di un flag di consenso esplicito per il test del nuovo comportamento delle specifiche.

### <a name="test-with-firefox"></a>Eseguire test con Firefox

Il supporto di Firefox per il nuovo standard può essere testato nella versione 68 + scegliendo nella pagina `about:config` con il flag funzionalità `network.cookie.sameSite.laxByDefault`. Non sono stati segnalati problemi di compatibilità con le versioni precedenti di Firefox.

### <a name="test-with-edge-legacy-browser"></a>Test con browser Edge (legacy)

Edge supporta lo standard navigava sullostesso sito precedente. Edge versione 44 + non presenta problemi di compatibilità noti con il nuovo standard.

### <a name="test-with-edge-chromium"></a>Test con Edge (cromo)

I flag navigava sullostesso sito sono impostati nella pagina `edge://flags/#same-site-by-default-cookies`. Nessun problema di compatibilità rilevato con cromo perimetrale.

### <a name="test-with-electron"></a>Test con elettrone

Le versioni di Electron includono versioni precedenti di cromo. Ad esempio, la versione di Electron usata dai team è Chromium 66, che presenta il comportamento precedente. È necessario eseguire test di compatibilità personalizzati con la versione di Electron utilizzata dal prodotto. Vedere [supporto di browser meno recenti](#sob).

## <a name="reverting-samesite-patches"></a>Ripristino di patch navigava sullostesso sito

È possibile ripristinare il comportamento di navigava sullostesso sito aggiornato nelle app .NET Framework al comportamento precedente in cui l'attributo navigava sullostesso sito non viene emesso per un valore di `None`e ripristinare i cookie di autenticazione e sessione in modo da non creare il valore. Questa operazione dovrebbe essere considerata una *correzione estremamente temporanea*, in quanto le modifiche al Chrome interromperanno le richieste post esterne o l'autenticazione per gli utenti che usano browser che supportano le modifiche allo standard.

### <a name="reverting-net-472-behavior"></a>Ripristino del comportamento 4.7.2 .NET

Aggiornare il *file Web. config* in modo da includere le impostazioni di configurazione seguenti:

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Prossime modifiche ai cookie navigava sullostesso sito in ASP.NET e ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Suggerimenti per il test e il debug di navigava sullostesso sito per impostazione predefinita e "navigava sullostesso sito = None; Proteggi "cookie](https://www.chromium.org/updates/same-site/test-debug)
* [Blog di Chromium: sviluppatori: prepararsi per la nuova navigava sullostesso sito = None; Impostazioni sicure del cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Spiegazione dei cookie navigava sullostesso sito](https://web.dev/samesite-cookies-explained/)
* [Aggiornamenti Chrome](https://www.chromium.org/updates/same-site)
* [Patch navigava sullostesso sito .NET](/aspnet/samesite/kbs-samesite)
* [Informazioni sullo stesso sito per le applicazioni Web di Azure](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [Informazioni sullo stesso sito di Azure ActiveDirectory](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
