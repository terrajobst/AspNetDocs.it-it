---
title: Esempio di cookie navigava sullostesso sito per Web C# form ASP.NET 4.7.2
author: blowdart
description: Esempio di cookie navigava sullostesso sito per Web C# form ASP.NET 4.7.2
ms.author: riande
ms.date: 2/15/2019
uid: samesite/CSharpWebForms
ms.openlocfilehash: 50d4745eca5954275abaa59dab726e7cf7ea193f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458483"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-webforms"></a><span data-ttu-id="0818e-103">Esempio di cookie navigava sullostesso sito per Web C# form ASP.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="0818e-103">SameSite cookie sample for ASP.NET 4.7.2 C# WebForms</span></span>

<span data-ttu-id="0818e-104">.NET Framework 4,7 dispone del supporto incorporato per l'attributo [navigava sullostesso sito](https://www.owasp.org/index.php/SameSite) , ma rispetta lo standard originale.</span><span class="sxs-lookup"><span data-stu-id="0818e-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="0818e-105">Il comportamento con patch ha modificato il significato di `SameSite.None` per emettere l'attributo con un valore di `None`, anziché non creare il valore.</span><span class="sxs-lookup"><span data-stu-id="0818e-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="0818e-106">Se non si desidera creare il valore, è possibile impostare la proprietà `SameSite` su un cookie su-1.</span><span class="sxs-lookup"><span data-stu-id="0818e-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="0818e-107">Scrittura dell'attributo navigava sullostesso sito</span><span class="sxs-lookup"><span data-stu-id="0818e-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="0818e-108">Di seguito è riportato un esempio di come scrivere un attributo navigava sullostesso sito in un cookie;</span><span class="sxs-lookup"><span data-stu-id="0818e-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookie
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for SameSite none.
// Note this will also require you to be running on HTTPS
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

<span data-ttu-id="0818e-109">L'attributo navigava sullostesso sito predefinito per un cookie di autenticazione basata su form viene impostato nel parametro `cookieSameSite` delle impostazioni di autenticazione basata su form in `web.config`</span><span class="sxs-lookup"><span data-stu-id="0818e-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="0818e-110">L'attributo navigava sullostesso sito predefinito per lo stato della sessione viene anche impostato nel parametro ' cookieSameSite ' delle impostazioni di sessione in `web.config`</span><span class="sxs-lookup"><span data-stu-id="0818e-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="0818e-111">L'aggiornamento di novembre 2019 a .NET ha modificato le impostazioni predefinite per l'autenticazione basata su form e la sessione `lax` come è l'impostazione più compatibile, tuttavia se si incorporano pagine in iframe, potrebbe essere necessario ripristinare questa impostazione su None, quindi aggiungere il codice di [intercettazione](#interception) riportato di seguito per modificare il comportamento del `none` a seconda della funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="0818e-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="0818e-112">Esecuzione dell'esempio</span><span class="sxs-lookup"><span data-stu-id="0818e-112">Running the sample</span></span>

<span data-ttu-id="0818e-113">Se si esegue il progetto di esempio, caricare il debugger del browser nella pagina iniziale e usarlo per visualizzare la raccolta di cookie per il sito.</span><span class="sxs-lookup"><span data-stu-id="0818e-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="0818e-114">A tale scopo, in Edge e Chrome premere `F12` quindi selezionare la scheda `Application` e fare clic sull'URL del sito sotto l'opzione `Cookies` nella sezione `Storage`.</span><span class="sxs-lookup"><span data-stu-id="0818e-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Elenco dei cookie del debugger del browser](sample/img/BrowserDebugger.png)

<span data-ttu-id="0818e-116">Nell'immagine precedente è possibile vedere che il cookie creato dall'esempio quando si fa clic sul pulsante "crea cookie" ha un valore dell'attributo navigava sullostesso sito di `Lax`, che corrisponde al valore impostato nel [codice di esempio](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="0818e-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="0818e-117">Intercettazione dei cookie che non si controllano</span><span class="sxs-lookup"><span data-stu-id="0818e-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="0818e-118">In .NET 4.5.2 è stato introdotto un nuovo evento per intercettare la scrittura delle intestazioni `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="0818e-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="0818e-119">Questa operazione può essere utilizzata per intercettare i cookie prima che vengano restituiti al computer client.</span><span class="sxs-lookup"><span data-stu-id="0818e-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="0818e-120">Nell'esempio viene associato l'evento a un metodo statico che controlla se il browser supporta le nuove modifiche di navigava sullostesso sito e, in caso contrario, modifica i cookie in modo da non creare l'attributo se è stato impostato il nuovo valore `None`.</span><span class="sxs-lookup"><span data-stu-id="0818e-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="0818e-121">Vedere [Global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) per un esempio di associazione dell'evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) per un esempio di gestione dell'evento e di modifica dell'attributo `sameSite` del cookie.</span><span class="sxs-lookup"><span data-stu-id="0818e-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>

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

<span data-ttu-id="0818e-122">È possibile modificare il comportamento specifico di un cookie denominato nello stesso modo; Nell'esempio seguente viene modificato il cookie di autenticazione predefinito da `Lax` a `None` sui browser che supportano il valore `None` o rimuove l'attributo navigava sullostesso sito nei browser che non supportano `None`.</span><span class="sxs-lookup"><span data-stu-id="0818e-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="0818e-123">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="0818e-123">More Information</span></span>

[<span data-ttu-id="0818e-124">Aggiornamenti Chrome</span><span class="sxs-lookup"><span data-stu-id="0818e-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="0818e-125">Documentazione di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0818e-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="0818e-126">Patch navigava sullostesso sito .NET</span><span class="sxs-lookup"><span data-stu-id="0818e-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)