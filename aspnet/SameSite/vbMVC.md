---
title: Esempio di cookie navigava sullostesso sito per ASP.NET 4.7.2 VB MVC
author: blowdart
description: Esempio di cookie navigava sullostesso sito per ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458469"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a><span data-ttu-id="00058-103">Esempio di cookie navigava sullostesso sito per ASP.NET 4.7.2 VB MVC</span><span class="sxs-lookup"><span data-stu-id="00058-103">SameSite cookie sample for ASP.NET 4.7.2 VB MVC</span></span>

<span data-ttu-id="00058-104">.NET Framework 4,7 dispone del supporto incorporato per l'attributo [navigava sullostesso sito](https://www.owasp.org/index.php/SameSite) , ma rispetta lo standard originale.</span><span class="sxs-lookup"><span data-stu-id="00058-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="00058-105">Il comportamento con patch ha modificato il significato di `SameSite.None` per emettere l'attributo con un valore di `None`, anziché non creare il valore.</span><span class="sxs-lookup"><span data-stu-id="00058-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="00058-106">Se non si desidera creare il valore, è possibile impostare la proprietà `SameSite` su un cookie su-1.</span><span class="sxs-lookup"><span data-stu-id="00058-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="00058-107">Scrittura dell'attributo navigava sullostesso sito</span><span class="sxs-lookup"><span data-stu-id="00058-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="00058-108">Di seguito è riportato un esempio di come scrivere un attributo navigava sullostesso sito in un cookie;</span><span class="sxs-lookup"><span data-stu-id="00058-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="00058-109">L'attributo navigava sullostesso sito predefinito per lo stato della sessione è impostato nel parametro ' cookieSameSite ' delle impostazioni di sessione in `web.config`</span><span class="sxs-lookup"><span data-stu-id="00058-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="00058-110">Autenticazione MVC</span><span class="sxs-lookup"><span data-stu-id="00058-110">MVC Authentication</span></span>

<span data-ttu-id="00058-111">L'autenticazione basata su cookie MVC OWIN usa un gestore di cookie per abilitare la modifica degli attributi dei cookie.</span><span class="sxs-lookup"><span data-stu-id="00058-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="00058-112">[SameSiteCookieManager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) è un'implementazione di una classe di questo tipo che può essere copiata nei propri progetti.</span><span class="sxs-lookup"><span data-stu-id="00058-112">The [SameSiteCookieManager.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="00058-113">È necessario assicurarsi che i componenti di Microsoft. Owin siano stati aggiornati alla versione 4.1.0 o successiva.</span><span class="sxs-lookup"><span data-stu-id="00058-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="00058-114">Controllare il file di `packages.config` per assicurarsi che tutti i numeri di versione corrispondano, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="00058-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

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

<span data-ttu-id="00058-115">I componenti di autenticazione devono essere configurati per l'uso di CookieManager nella classe Startup.</span><span class="sxs-lookup"><span data-stu-id="00058-115">The authentication components must be configured to use the CookieManager in your startup class;</span></span>

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

<span data-ttu-id="00058-116">È necessario impostare un gestore di cookie in *ogni* componente che lo supporta, inclusi CookieAuthentication e OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="00058-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="00058-117">SystemWebCookieManager viene usato per evitare [problemi noti relativi](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) all'integrazione dei cookie di risposta.</span><span class="sxs-lookup"><span data-stu-id="00058-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="00058-118">Esecuzione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="00058-118">Running the sample</span></span>

<span data-ttu-id="00058-119">Se si esegue il progetto di esempio, caricare il debugger del browser nella pagina iniziale e usarlo per visualizzare la raccolta di cookie per il sito.</span><span class="sxs-lookup"><span data-stu-id="00058-119">If you run the sample project please load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="00058-120">A tale scopo, in Edge e Chrome premere `F12` quindi selezionare la scheda `Application` e fare clic sull'URL del sito sotto l'opzione `Cookies` nella sezione `Storage`.</span><span class="sxs-lookup"><span data-stu-id="00058-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Elenco dei cookie del debugger del browser](sample/img/BrowserDebugger.png)

<span data-ttu-id="00058-122">Nell'immagine precedente è possibile vedere che il cookie creato dall'esempio quando si fa clic sul pulsante "create navigava sullostesso sito cookie" ha un valore di attributo navigava sullostesso sito pari a `Lax`, che corrisponde al valore impostato nel [codice di esempio](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="00058-122">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="00058-123">Intercettazione dei cookie che non si controllano</span><span class="sxs-lookup"><span data-stu-id="00058-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="00058-124">In .NET 4.5.2 è stato introdotto un nuovo evento per intercettare la scrittura delle intestazioni `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="00058-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="00058-125">Questa operazione può essere utilizzata per intercettare i cookie prima che vengano restituiti al computer client.</span><span class="sxs-lookup"><span data-stu-id="00058-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="00058-126">Nell'esempio viene associato l'evento a un metodo statico che controlla se il browser supporta le nuove modifiche di navigava sullostesso sito e, in caso contrario, modifica i cookie in modo da non creare l'attributo se è stato impostato il nuovo valore `None`.</span><span class="sxs-lookup"><span data-stu-id="00058-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="00058-127">Vedere [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) per un esempio di associazione dell'evento e di [SameSiteCookieRewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) per un esempio di gestione dell'evento e di modifica dell'attributo `sameSite` cookie che è possibile copiare nel codice.</span><span class="sxs-lookup"><span data-stu-id="00058-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

<span data-ttu-id="00058-128">È possibile modificare il comportamento specifico di un cookie denominato nello stesso modo; Nell'esempio seguente viene modificato il cookie di autenticazione predefinito da `Lax` a `None` sui browser che supportano il valore `None` o rimuove l'attributo navigava sullostesso sito nei browser che non supportano `None`.</span><span class="sxs-lookup"><span data-stu-id="00058-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a><span data-ttu-id="00058-129">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="00058-129">More Information</span></span>
 
[<span data-ttu-id="00058-130">Aggiornamenti Chrome</span><span class="sxs-lookup"><span data-stu-id="00058-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="00058-131">Documentazione di OWIN navigava sullostesso sito</span><span class="sxs-lookup"><span data-stu-id="00058-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="00058-132">Documentazione di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00058-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="00058-133">Patch navigava sullostesso sito .NET</span><span class="sxs-lookup"><span data-stu-id="00058-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)