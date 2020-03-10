---
title: Esempio di cookie navigava sullostesso sito per ASP.NET C# 4.7.2 MVC
author: blowdart
description: Esempio di cookie navigava sullostesso sito per ASP.NET C# 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544723"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a>Esempio di cookie navigava sullostesso sito per ASP.NET C# 4.7.2 MVC

.NET Framework 4,7 dispone del supporto incorporato per l'attributo [navigava sullostesso sito](https://www.owasp.org/index.php/SameSite) , ma rispetta lo standard originale.
Il comportamento con patch ha modificato il significato di `SameSite.None` per emettere l'attributo con un valore di `None`, anziché non creare il valore. Se non si desidera creare il valore, è possibile impostare la proprietà `SameSite` su un cookie su-1.

## <a name="sampleCode"></a>Scrittura dell'attributo navigava sullostesso sito

Di seguito è riportato un esempio di come scrivere un attributo navigava sullostesso sito in un cookie;

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

L'attributo navigava sullostesso sito predefinito per lo stato della sessione è impostato nel parametro ' cookieSameSite ' delle impostazioni di sessione in `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Autenticazione MVC

L'autenticazione basata su cookie MVC OWIN usa un gestore di cookie per abilitare la modifica degli attributi dei cookie. [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) è un'implementazione di una classe di questo tipo che può essere copiata nei propri progetti. 

È necessario assicurarsi che i componenti di Microsoft. Owin siano stati aggiornati alla versione 4.1.0 o successiva. Controllare il file di `packages.config` per assicurarsi che tutti i numeri di versione corrispondano, ad esempio.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

I componenti di autenticazione devono quindi essere configurati per l'uso di CookieManager nella classe Startup.

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

È necessario impostare un gestore di cookie in *ogni* componente che lo supporta, inclusi CookieAuthentication e OpenIdConnectAuthentication.

SystemWebCookieManager viene usato per evitare [problemi noti relativi](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) all'integrazione dei cookie di risposta.

### <a name="running-the-sample"></a>Esecuzione dell'esempio

Se si esegue il progetto di esempio, caricare il debugger del browser nella pagina iniziale e usarlo per visualizzare la raccolta di cookie per il sito.
A tale scopo, in Edge e Chrome premere `F12` quindi selezionare la scheda `Application` e fare clic sull'URL del sito sotto l'opzione `Cookies` nella sezione `Storage`.

![Elenco dei cookie del debugger del browser](sample/img/BrowserDebugger.png)

Nell'immagine precedente è possibile vedere che il cookie creato dall'esempio quando si fa clic sul pulsante "crea cookie" ha un valore dell'attributo navigava sullostesso sito di `Lax`, che corrisponde al valore impostato nel [codice di esempio](#sampleCode).

## <a name="interception"></a>Intercettazione dei cookie che non si controllano

In .NET 4.5.2 è stato introdotto un nuovo evento per intercettare la scrittura delle intestazioni `Response.AddOnSendingHeaders`. Questa operazione può essere utilizzata per intercettare i cookie prima che vengano restituiti al computer client. Nell'esempio viene associato l'evento a un metodo statico che controlla se il browser supporta le nuove modifiche di navigava sullostesso sito e, in caso contrario, modifica i cookie in modo da non creare l'attributo se è stato impostato il nuovo valore `None`.

Vedere [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) per un esempio di come associare l'evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) per un esempio di gestione dell'evento e la modifica dell'attributo `sameSite` cookie che è possibile copiare nel codice personalizzato.

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

È possibile modificare il comportamento specifico di un cookie denominato nello stesso modo; Nell'esempio seguente viene modificato il cookie di autenticazione predefinito da `Lax` a `None` sui browser che supportano il valore `None` o rimuove l'attributo navigava sullostesso sito nei browser che non supportano `None`.

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a>Altre informazioni
 
[Aggiornamenti Chrome](https://www.chromium.org/updates/same-site)

[Documentazione di OWIN navigava sullostesso sito](/aspnet/samesite/owin-samesite)

[Documentazione di ASP.NET](/aspnet/samesite/system-web-samesite)

[Patch navigava sullostesso sito .NET](/aspnet/samesite/kbs-samesite)