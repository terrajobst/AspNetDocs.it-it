---
uid: web-api/overview/security/basic-authentication
title: Autenticazione di base in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione di base in API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555727"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="bf907-103">Autenticazione di base in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bf907-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="bf907-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf907-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bf907-105">L'autenticazione di base è definita in [RFC 2617, autenticazione http: autenticazione dell'accesso di base e digest](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="bf907-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="bf907-106">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="bf907-106">Disadvantages</span></span>

- <span data-ttu-id="bf907-107">Le credenziali utente vengono inviate nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="bf907-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="bf907-108">Le credenziali vengono inviate come testo non crittografato.</span><span class="sxs-lookup"><span data-stu-id="bf907-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="bf907-109">Le credenziali vengono inviate con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="bf907-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="bf907-110">Non è possibile disconnettersi, tranne terminando la sessione del browser.</span><span class="sxs-lookup"><span data-stu-id="bf907-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="bf907-111">Vulnerabile a richieste intersito false (CSRF); richiede misure anti-CSRF.</span><span class="sxs-lookup"><span data-stu-id="bf907-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="bf907-112">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="bf907-112">Advantages</span></span>

- <span data-ttu-id="bf907-113">Internet standard.</span><span class="sxs-lookup"><span data-stu-id="bf907-113">Internet standard.</span></span>
- <span data-ttu-id="bf907-114">Supportato da tutti i browser principali.</span><span class="sxs-lookup"><span data-stu-id="bf907-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="bf907-115">Protocollo relativamente semplice.</span><span class="sxs-lookup"><span data-stu-id="bf907-115">Relatively simple protocol.</span></span>

<span data-ttu-id="bf907-116">L'autenticazione di base funziona nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="bf907-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="bf907-117">Se per una richiesta è necessaria l'autenticazione, il server restituisce 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="bf907-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="bf907-118">La risposta include un'intestazione WWW-Authenticate, che indica che il server supporta l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="bf907-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="bf907-119">Il client invia un'altra richiesta con le credenziali client nell'intestazione dell'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="bf907-120">Le credenziali vengono formattate come stringa "Name: password", con codifica Base64.</span><span class="sxs-lookup"><span data-stu-id="bf907-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="bf907-121">Le credenziali non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="bf907-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="bf907-122">L'autenticazione di base viene eseguita nel contesto di un'area di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="bf907-123">Il server include il nome dell'area di autenticazione nell'intestazione WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="bf907-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="bf907-124">Le credenziali dell'utente sono valide all'interno dell'area di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="bf907-125">L'ambito esatto di un'area di autenticazione è definito dal server.</span><span class="sxs-lookup"><span data-stu-id="bf907-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="bf907-126">È ad esempio possibile definire più aree di autenticazione per partizionare le risorse.</span><span class="sxs-lookup"><span data-stu-id="bf907-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="bf907-127">Poiché le credenziali vengono inviate senza crittografia, l'autenticazione di base è protetta solo tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf907-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="bf907-128">Vedere [uso di SSL nell'API Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bf907-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="bf907-129">L'autenticazione di base è anche vulnerabile agli attacchi CSRF.</span><span class="sxs-lookup"><span data-stu-id="bf907-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="bf907-130">Dopo l'immissione delle credenziali da parte dell'utente, il browser le invia automaticamente alle richieste successive allo stesso dominio per la durata della sessione.</span><span class="sxs-lookup"><span data-stu-id="bf907-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="bf907-131">Sono incluse le richieste AJAX.</span><span class="sxs-lookup"><span data-stu-id="bf907-131">This includes AJAX requests.</span></span> <span data-ttu-id="bf907-132">Vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="bf907-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="bf907-133">Autenticazione di base con IIS</span><span class="sxs-lookup"><span data-stu-id="bf907-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="bf907-134">IIS supporta l'autenticazione di base, ma è presente un avvertimento: l'utente viene autenticato con le credenziali di Windows.</span><span class="sxs-lookup"><span data-stu-id="bf907-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="bf907-135">Ciò significa che l'utente deve disporre di un account nel dominio del server.</span><span class="sxs-lookup"><span data-stu-id="bf907-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="bf907-136">Per un sito Web pubblico, in genere si desidera eseguire l'autenticazione in base a un provider di appartenenze ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf907-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="bf907-137">Per abilitare l'autenticazione di base usando IIS, impostare la modalità di autenticazione su "Windows" nel file Web. config del progetto ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="bf907-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="bf907-138">In questa modalità, IIS utilizza le credenziali di Windows per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="bf907-139">Inoltre, è necessario abilitare l'autenticazione di base in IIS.</span><span class="sxs-lookup"><span data-stu-id="bf907-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="bf907-140">In Gestione IIS passare a visualizzazione funzionalità, selezionare autenticazione e Abilita autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="bf907-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="bf907-141">Nel progetto API Web aggiungere l'attributo `[Authorize]` per le azioni del controller che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="bf907-142">Un client esegue l'autenticazione impostando l'intestazione di autorizzazione nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="bf907-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="bf907-143">I client del browser eseguono questo passaggio automaticamente.</span><span class="sxs-lookup"><span data-stu-id="bf907-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="bf907-144">I client non browser dovranno impostare l'intestazione.</span><span class="sxs-lookup"><span data-stu-id="bf907-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="bf907-145">Autenticazione di base con appartenenza personalizzata</span><span class="sxs-lookup"><span data-stu-id="bf907-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="bf907-146">Come indicato in precedenza, l'autenticazione di base incorporata in IIS utilizza le credenziali di Windows.</span><span class="sxs-lookup"><span data-stu-id="bf907-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="bf907-147">Ciò significa che è necessario creare account per gli utenti nel server di hosting.</span><span class="sxs-lookup"><span data-stu-id="bf907-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="bf907-148">Per un'applicazione Internet, tuttavia, gli account utente vengono in genere archiviati in un database esterno.</span><span class="sxs-lookup"><span data-stu-id="bf907-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="bf907-149">Il codice seguente illustra come un modulo HTTP che esegue l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="bf907-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="bf907-150">È possibile inserire facilmente un provider di appartenenze ASP.NET sostituendo il metodo `CheckPassword`, che in questo esempio è un metodo fittizio.</span><span class="sxs-lookup"><span data-stu-id="bf907-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="bf907-151">Nell'API Web 2 è consigliabile scrivere un filtro di [autenticazione](authentication-filters.md) o un [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md)anziché un modulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="bf907-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="bf907-152">Per abilitare il modulo HTTP, aggiungere il codice seguente al file Web. config nella sezione **System. webserver** :</span><span class="sxs-lookup"><span data-stu-id="bf907-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="bf907-153">Sostituire "YourAssemblyName" con il nome dell'assembly (esclusa l'estensione "dll").</span><span class="sxs-lookup"><span data-stu-id="bf907-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="bf907-154">È necessario disabilitare altri schemi di autenticazione, ad esempio moduli o autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bf907-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
