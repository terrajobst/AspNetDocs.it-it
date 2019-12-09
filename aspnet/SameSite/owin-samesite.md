---
title: Usare i cookie navigava sullostesso sito e l'interfaccia Web aperta per .NET (OWIN)
author: rick-anderson
description: Usare i cookie navigava sullostesso sito e l'interfaccia Web aperta per .NET (OWIN)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: fc64315e8c3614e460c9a8d551bcb0848b3fe8f9
ms.sourcegitcommit: 516a168548252ff0eaae2c02ec4bd9ffcfa8375e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951896"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>Cookie navigava sullostesso sito e Open Web Interface for .NET (OWIN)

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` è una bozza [IETF](https://ietf.org/about/) progettata per garantire una protezione contro gli attacchi di richiesta intersito falsa (CSRF). [Bozza di navigava sullostesso sito 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Considera i cookie come `SameSite=Lax` per impostazione predefinita.
* Indica che i cookie che asseriscono in modo esplicito `SameSite=None` per abilitare il recapito tra siti devono essere contrassegnati come `Secure`.

`Lax` funziona per la maggior parte dei cookie dell'app. Alcune forme di autenticazione come [OpenID Connect](https://openid.net/connect/) (OIDC) e l'impostazione predefinita di [WS-Federation](https://auth0.com/docs/protocols/ws-fed) sono i reindirizzamenti basati su post. I reindirizzamenti basati su POST attivano le protezioni del browser `SameSite`, quindi `SameSite` è disabilitato per questi componenti. La maggior parte degli accessi [OAuth](https://oauth.net/) non è interessata a causa di differenze nel modo in cui la richiesta fluisce. Tutti gli altri componenti **non** impostano `SameSite` per impostazione predefinita e utilizzano il comportamento predefinito dei client (vecchio o nuovo).

Il parametro `None` causa problemi di compatibilità con i client che hanno implementato lo [standard Draft precedente 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (ad esempio, iOS 12). Vedere [supporto di browser meno recenti](#sob) in questo documento.

Ogni componente OWIN che emette cookie deve decidere se `SameSite` è appropriato.

Per la versione 4. x di ASP.NET di questo articolo, vedere <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>Utilizzo delle API con navigava sullostesso sito

`Microsoft.Owin` dispone di una propria implementazione di `SameSite`:

* Che non dipende direttamente da quello in `System.Web`.
* `SameSite` funziona con tutte le versioni destinate ai pacchetti di `Microsoft.Owin`, .NET 4,5 e versioni successive.
* Solo il componente [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) interagisce direttamente con la classe `System.Web` `HttpCookie`.

`SystemWebCookieManager` dipende dalle API .NET 4.7.2 `System.Web` per abilitare il supporto `SameSite` e le patch per modificare il comportamento.

I motivi per utilizzare `SystemWebCookieManager` sono descritti nei problemi di [integrazione dei cookie di risposta OWIN e System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues). Quando si esegue in `System.Web`, è consigliabile `SystemWebCookieManager`. 

Il codice seguente imposta `SameSite` `Lax`:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

Le API seguenti usano `SameSite`:

* [Microsoft. Owin. SameSiteMode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [CookieOptions. navigava sullostesso sito](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Classe CookieAuthenticationOptions](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [CookieAuthenticationOptions. CookieSameSite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [SystemWebChunkingCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [CookieAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [OpenIdConnectAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Cronologia e modifiche

[Microsoft. Owin](https://www.nuget.org/packages/Microsoft.Owin/) non supporta mai lo [standard`SameSite` 2016 Draft](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Il supporto per la [bozza navigava sullostesso sito 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) è disponibile solo in `Microsoft.Owin` 4.1.0 e versioni successive. Non sono presenti patch per le versioni precedenti.

Bozza 2019 della specifica `SameSite`:

* **Non** è compatibile con le versioni precedenti della bozza 2016. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.
* Specifica che i cookie vengono considerati come `SameSite=Lax` per impostazione predefinita.
* Specifica i cookie che asseriscono in modo esplicito `SameSite=None` per consentire il recapito tra siti deve essere contrassegnato come `Secure`. `None` è una nuova voce da rifiutare esplicitamente.
* Viene pianificata per essere abilitata per impostazione predefinita da [Chrome](https://chromestatus.com/feature/5088147346030592) in [feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Il passaggio a questo standard nei browser è stato avviato in 2019.
* È supportato dalle patch rilasciate come descritto nei seguenti KB:
  * [Articolo della Knowledge 4531182](https://support.microsoft.com/help/4531182/kb4531182)
  * [Articolo della Knowledge 4524421](https://support.microsoft.com/help/4524421/kb4524421)

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Supporto di browser meno recenti

2016 `SameSite` standard imposto che i valori sconosciuti devono essere considerati come `SameSite=Strict` valori. Le app a cui è stato eseguito l'accesso da browser meno recenti che supportano lo standard 2016 `SameSite` possono interrompersi quando ottengono una proprietà `SameSite` con un valore di `None`. Le app Web devono implementare il rilevamento del browser se intendono supportare browser meno recenti. ASP.NET non implementa il rilevamento del browser perché i valori degli agenti utente sono altamente volatili e cambiano di frequente. Un punto di estensione in [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) consente di collegare la logica specifica dell'agente utente.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

In `Startup.Configuration`aggiungere codice simile al seguente:

[!code-csharp[](sample/Startup1.cs?name=snippet)]

Il codice precedente richiede .NET 4.7.2 o versioni successive `SameSite` patch.

Nel codice seguente viene illustrata un'implementazione di esempio di `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

Nell'esempio precedente, `DisallowsSameSiteNone` viene chiamato nel metodo `CheckSameSite`. `DisallowsSameSiteNone` è un metodo utente che rileva se l'agente utente non supporta `SameSite` `None`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

Il codice seguente illustra un metodo di `DisallowsSameSiteNone` di esempio:

> [!WARNING]
> Il codice seguente è solo a scopo dimostrativo:
> * Non deve essere considerato completo.
> * Non è gestita né supportata.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testare le app per i problemi di navigava sullostesso sito

Le app che interagiscono con i siti remoti, ad esempio tramite l'accesso di terze parti, devono:

* Testare l'interazione su più browser.
* Applicare il [rilevamento e la mitigazione del browser](#sob) discussi in questo documento.

Testare le app Web usando una versione client che può acconsentire esplicitamente al nuovo comportamento del `SameSite`. Chrome, Firefox e Chromium Edge hanno tutti nuovi flag di funzionalità di consenso esplicito che possono essere usati per i test. Dopo che l'app applica le patch `SameSite`, testarla con versioni client meno recenti, in particolare Safari. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.

### <a name="test-with-chrome"></a>Eseguire test con Chrome

Chrome 78 + fornisce risultati fuorvianti perché prevede una mitigazione temporanea. La mitigazione temporanea di Chrome 78 + consente ai cookie meno di due minuti di età. Chrome 76 o 77 con i flag di test appropriati abilitati fornisce risultati più accurati. Per testare il nuovo comportamento del `SameSite` attivare o disabilitare `chrome://flags/#same-site-by-default-cookies` su **abilitato**. Le versioni precedenti di Chrome (75 e versioni precedenti) vengono segnalate per avere esito negativo con la nuova impostazione `None`. Vedere [supporto di browser meno recenti](#sob) in questo documento.

Google non rende disponibili versioni precedenti di Chrome. Seguire le istruzioni riportate in [scaricare Chromium](https://www.chromium.org/getting-involved/download-chromium) per testare le versioni precedenti di Chrome. Non **scaricare Chrome** dai collegamenti forniti cercando le versioni precedenti di Chrome.

* [Cromo 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Cromo 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Eseguire test con Safari

Safari 12 ha implementato rigorosamente la bozza precedente e ha esito negativo quando il nuovo valore `None` si trova in un cookie. `None` viene evitata tramite il codice di rilevamento del browser che [supporta i browser meno recenti](#sob) in questo documento. Testare gli accessi basati su stile sistema operativo Safari 12, Safari 13 e WebKit usando MSAL, ADAL o qualsiasi libreria in uso. Il problema dipende dalla versione del sistema operativo sottostante. OSX Mojave (10,14) e iOS 12 sono noti per problemi di compatibilità con il nuovo comportamento `SameSite`. L'aggiornamento del sistema operativo a OSX Catalina (10,15) o iOS 13 corregge il problema. Safari attualmente non dispone di un flag di consenso esplicito per il test del nuovo comportamento delle specifiche.

### <a name="test-with-firefox"></a>Eseguire test con Firefox

Il supporto di Firefox per il nuovo standard può essere testato nella versione 68 + scegliendo nella pagina `about:config` con il flag funzionalità `network.cookie.sameSite.laxByDefault`. Non sono stati segnalati problemi di compatibilità con le versioni precedenti di Firefox.

### <a name="test-with-edge-browser"></a>Eseguire test con il browser Microsoft Edge

Edge supporta il vecchio `SameSite` standard. La versione perimetrale 44 non presenta problemi di compatibilità noti con il nuovo standard.

### <a name="test-with-edge-chromium"></a>Test con Edge (cromo)

i flag di `SameSite` vengono impostati nella pagina `edge://flags/#same-site-by-default-cookies`. Nessun problema di compatibilità rilevato con cromo perimetrale.

### <a name="test-with-electron"></a>Test con elettrone

Le versioni di Electron includono versioni precedenti di cromo. Ad esempio, la versione di Electron usata dai team è Chromium 66, che presenta il comportamento precedente. È necessario eseguire test di compatibilità personalizzati con la versione di Electron utilizzata dal prodotto. Vedere [supporto dei browser precedenti](#sob) nella sezione seguente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Blog di Chromium: sviluppatori: prepararsi per la nuova navigava sullostesso sito = None; Impostazioni sicure del cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Spiegazione dei cookie navigava sullostesso sito](https://web.dev/samesite-cookies-explained/)
* [Problemi di integrazione dei cookie di risposta OWIN e System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)
