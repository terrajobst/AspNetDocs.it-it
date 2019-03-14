---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevenzione degli attacchi di falsificazione (CSRF) richiesta tra siti in ASP.NET MVC
author: MikeWasson
description: Viene descritto come implementare misure anti-CSRF in ASP.NET Web MVC e l'attacco di falsificazione (CSRF) richiesta tra siti.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: c88975d1c205e9d0733bfb4c710b92bc8fdaaa7a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042368"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a><span data-ttu-id="4b066-103">Prevenzione degli attacchi di falsificazione (CSRF) richiesta tra siti nell'applicazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4b066-103">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="4b066-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b066-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4b066-105">Cross-Site richiesta intersito falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso</span><span class="sxs-lookup"><span data-stu-id="4b066-105">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in</span></span>

<span data-ttu-id="4b066-106">Di seguito è riportato un esempio di un attacco CSRF:</span><span class="sxs-lookup"><span data-stu-id="4b066-106">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="4b066-107">Un utente accede a `www.example.com` tramite autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="4b066-107">A user logs into `www.example.com` using forms authentication.</span></span>
2. <span data-ttu-id="4b066-108">Il server autentica l'utente.</span><span class="sxs-lookup"><span data-stu-id="4b066-108">The server authenticates the user.</span></span> <span data-ttu-id="4b066-109">La risposta dal server include un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4b066-109">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="4b066-110">Senza la disconnessione, l'utente visita un sito web dannoso.</span><span class="sxs-lookup"><span data-stu-id="4b066-110">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="4b066-111">Questo sito dannoso contiene il form HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="4b066-111">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    <span data-ttu-id="4b066-112">Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso.</span><span class="sxs-lookup"><span data-stu-id="4b066-112">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="4b066-113">Questa è la parte "intersito" di CSRF.</span><span class="sxs-lookup"><span data-stu-id="4b066-113">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="4b066-114">L'utente fa clic sul pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="4b066-114">The user clicks the submit button.</span></span> <span data-ttu-id="4b066-115">Il visualizzatore include il cookie di autenticazione con la richiesta.</span><span class="sxs-lookup"><span data-stu-id="4b066-115">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="4b066-116">La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che un utente autenticato può eseguire.</span><span class="sxs-lookup"><span data-stu-id="4b066-116">The request runs on the server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="4b066-117">Anche se in questo esempio richiede all'utente di fare clic sul pulsante del form, la pagina dannosa potrebbe eseguite facilmente uno script che invia il form automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4b066-117">Although this example requires the user to click the form button, the malicious page could just as easily run a script that submits the form automatically.</span></span> <span data-ttu-id="4b066-118">Inoltre, con SSL non impedire un attacco CSRF, poiché il sito dannoso può inviare una richiesta di "https://".</span><span class="sxs-lookup"><span data-stu-id="4b066-118">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="4b066-119">In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito web di destinazione.</span><span class="sxs-lookup"><span data-stu-id="4b066-119">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="4b066-120">Attacchi CSRF, tuttavia, non sono limitati a sfruttare i cookie.</span><span class="sxs-lookup"><span data-stu-id="4b066-120">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="4b066-121">Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="4b066-121">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="4b066-122">Dopo un utente accede con l'autenticazione di base o Digest.</span><span class="sxs-lookup"><span data-stu-id="4b066-122">After a user logs in with Basic or Digest authentication.</span></span> <span data-ttu-id="4b066-123">il browser invia automaticamente le credenziali fino a quando non termina la sessione.</span><span class="sxs-lookup"><span data-stu-id="4b066-123">the browser automatically sends the credentials until the session ends.</span></span>

## <a name="anti-forgery-tokens"></a><span data-ttu-id="4b066-124">Token antifalsificazione</span><span class="sxs-lookup"><span data-stu-id="4b066-124">Anti-Forgery Tokens</span></span>

<span data-ttu-id="4b066-125">Per prevenire attacchi CSRF, ASP.NET MVC Usa i token antifalsificazione, chiamati anche *richiedere i token di verifica*.</span><span class="sxs-lookup"><span data-stu-id="4b066-125">To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called *request verification tokens*.</span></span>

1. <span data-ttu-id="4b066-126">Il client richiede una pagina HTML che contiene un modulo.</span><span class="sxs-lookup"><span data-stu-id="4b066-126">The client requests an HTML page that contains a form.</span></span>
2. <span data-ttu-id="4b066-127">Il server include due token nella risposta.</span><span class="sxs-lookup"><span data-stu-id="4b066-127">The server includes two tokens in the response.</span></span> <span data-ttu-id="4b066-128">Un token viene inviato come cookie.</span><span class="sxs-lookup"><span data-stu-id="4b066-128">One token is sent as a cookie.</span></span> <span data-ttu-id="4b066-129">L'altra viene inserita nel campo del form nascosto.</span><span class="sxs-lookup"><span data-stu-id="4b066-129">The other is placed in a hidden form field.</span></span> <span data-ttu-id="4b066-130">I token vengono generati in modo casuale, in modo che un utente malintenzionato non può dedurre i valori.</span><span class="sxs-lookup"><span data-stu-id="4b066-130">The tokens are generated randomly so that an adversary cannot guess the values.</span></span>
3. <span data-ttu-id="4b066-131">Quando il client invia il form, è necessario inviare entrambi i token al server.</span><span class="sxs-lookup"><span data-stu-id="4b066-131">When the client submits the form, it must send both tokens back to the server.</span></span> <span data-ttu-id="4b066-132">Il client invia il token del cookie come cookie e invia il token del form all'interno di dati del form.</span><span class="sxs-lookup"><span data-stu-id="4b066-132">The client sends the cookie token as a cookie, and it sends the form token inside the form data.</span></span> <span data-ttu-id="4b066-133">(Un client browser esegue automaticamente questa quando l'utente invia il form.)</span><span class="sxs-lookup"><span data-stu-id="4b066-133">(A browser client automatically does this when the user submits the form.)</span></span>
4. <span data-ttu-id="4b066-134">Se una richiesta non include entrambi i token, il server non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="4b066-134">If a request does not include both tokens, the server disallows the request.</span></span>

<span data-ttu-id="4b066-135">Ecco un esempio di un form HTML con un token del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="4b066-135">Here is an example of an HTML form with a hidden form token:</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

<span data-ttu-id="4b066-136">I token antifalsificazione funzionano perché la pagina non è in grado di leggere i token dell'utente, a causa dei criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="4b066-136">Anti-forgery tokens work because the malicious page cannot read the user's tokens, due to same-origin policies.</span></span> <span data-ttu-id="4b066-137">([i criteri di corrispondenza dell'origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedire che i documenti ospitati in due siti diversi l'accesso al contenuto di altro.</span><span class="sxs-lookup"><span data-stu-id="4b066-137">([Same-origin policies](http://www.w3.org/Security/wiki/Same_Origin_Policy) prevent documents hosted on two different sites from accessing each other's content.</span></span> <span data-ttu-id="4b066-138">Nell'esempio precedente, la pagina dannosa può inviare richieste all'example.com, ma che non è possibile leggere la risposta.)</span><span class="sxs-lookup"><span data-stu-id="4b066-138">So in the earlier example, the malicious page can send requests to example.com, but it cannot read the response.)</span></span>

<span data-ttu-id="4b066-139">Per impedire attacchi CSRF, usare i token antifalsificazione con qualsiasi protocollo di autenticazione in cui il browser invia automaticamente le credenziali dopo l'accesso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="4b066-139">To prevent CSRF attacks, use anti-forgery tokens with any authentication protocol where the browser silently sends credentials after the user logs in.</span></span> <span data-ttu-id="4b066-140">Questo include i protocolli di autenticazione basata su cookie, ad esempio autenticazione basata su form, nonché i protocolli, ad esempio autenticazione di base e Digest.</span><span class="sxs-lookup"><span data-stu-id="4b066-140">This includes cookie-based authentication protocols, such as forms authentication, as well as protocols such as Basic and Digest authentication.</span></span>

<span data-ttu-id="4b066-141">È consigliabile richiedere i token antifalsificazione per alcun metodo nonsafe (POST, PUT, DELETE).</span><span class="sxs-lookup"><span data-stu-id="4b066-141">You should require anti-forgery tokens for any nonsafe methods (POST, PUT, DELETE).</span></span> <span data-ttu-id="4b066-142">Inoltre, assicurarsi che non sicuri metodi (GET, HEAD) dispone tutti gli effetti collaterali.</span><span class="sxs-lookup"><span data-stu-id="4b066-142">Also, make sure that safe methods (GET, HEAD) do not have any side effects.</span></span> <span data-ttu-id="4b066-143">Inoltre, se si abilita il supporto di domini, ad esempio, JSONP o CORS sicuri anche metodi come GET sono potenzialmente vulnerabili agli attacchi CSRF, consentendo all'utente malintenzionato di leggere i dati potenzialmente sensibili.</span><span class="sxs-lookup"><span data-stu-id="4b066-143">Moreover, if you enable cross-domain support, such as CORS or JSONP, then even safe methods like GET are potentially vulnerable to CSRF attacks, allowing the attacker to read potentially sensitive data.</span></span>

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a><span data-ttu-id="4b066-144">Token antifalsificazione in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4b066-144">Anti-Forgery Tokens in ASP.NET MVC</span></span>

<span data-ttu-id="4b066-145">Per aggiungere i token antifalsificazione in una pagina Razor, usare il **HtmlHelper.AntiForgeryToken** metodo helper:</span><span class="sxs-lookup"><span data-stu-id="4b066-145">To add the anti-forgery tokens to a Razor page, use the **HtmlHelper.AntiForgeryToken** helper method:</span></span>

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

<span data-ttu-id="4b066-146">Questo metodo aggiunge il campo del form nascosto e imposta anche il token del cookie.</span><span class="sxs-lookup"><span data-stu-id="4b066-146">This method adds the hidden form field and also sets the cookie token.</span></span>

## <a name="anti-csrf-and-ajax"></a><span data-ttu-id="4b066-147">AJAX e Intersito</span><span class="sxs-lookup"><span data-stu-id="4b066-147">Anti-CSRF and AJAX</span></span>

<span data-ttu-id="4b066-148">Il token del form può rappresentare un problema per le richieste AJAX, perché una richiesta AJAX potrebbe inviare dati JSON, non i dati di form HTML.</span><span class="sxs-lookup"><span data-stu-id="4b066-148">The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="4b066-149">Una soluzione consiste nell'inviare i token in un'intestazione HTTP personalizzata.</span><span class="sxs-lookup"><span data-stu-id="4b066-149">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="4b066-150">Il codice seguente usa la sintassi Razor per generare i token e quindi li aggiunge a una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="4b066-150">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> <span data-ttu-id="4b066-151">I token vengono generati nel server chiamando **AntiForgery.GetTokens**.</span><span class="sxs-lookup"><span data-stu-id="4b066-151">The tokens are generated at the server by calling **AntiForgery.GetTokens**.</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

<span data-ttu-id="4b066-152">Quando si elabora la richiesta, estrarre i token dall'intestazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="4b066-152">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="4b066-153">Chiamare quindi il **Antiforgery** metodo per convalidare i token.</span><span class="sxs-lookup"><span data-stu-id="4b066-153">Then call the **AntiForgery.Validate** method to validate the tokens.</span></span> <span data-ttu-id="4b066-154">Il **Validate** metodo genera un'eccezione se il token non sono validi.</span><span class="sxs-lookup"><span data-stu-id="4b066-154">The **Validate** method throws an exception if the tokens are not valid.</span></span>

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]