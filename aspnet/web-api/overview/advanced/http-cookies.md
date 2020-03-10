---
uid: web-api/overview/advanced/http-cookies
title: Cookie HTTP in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Viene descritto come inviare e ricevere cookie HTTP nell'API Web per ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557687"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="f76c7-103">Cookie HTTP nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f76c7-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="f76c7-104">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f76c7-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f76c7-105">In questo argomento viene descritto come inviare e ricevere cookie HTTP nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f76c7-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="f76c7-106">Background su cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="f76c7-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="f76c7-107">Questa sezione fornisce una breve panoramica della modalità di implementazione dei cookie a livello HTTP.</span><span class="sxs-lookup"><span data-stu-id="f76c7-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="f76c7-108">Per informazioni dettagliate, vedere [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="f76c7-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="f76c7-109">Un cookie è una parte di dati che un server invia nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f76c7-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="f76c7-110">Il client (facoltativamente) archivia il cookie e lo restituisce alle richieste successive.</span><span class="sxs-lookup"><span data-stu-id="f76c7-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="f76c7-111">Ciò consente al client e al server di condividere lo stato.</span><span class="sxs-lookup"><span data-stu-id="f76c7-111">This allows the client and server to share state.</span></span> <span data-ttu-id="f76c7-112">Per impostare un cookie, il server include un'intestazione set-cookie nella risposta.</span><span class="sxs-lookup"><span data-stu-id="f76c7-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="f76c7-113">Il formato di un cookie è una coppia nome-valore, con attributi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="f76c7-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="f76c7-114">Esempio:</span><span class="sxs-lookup"><span data-stu-id="f76c7-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="f76c7-115">Di seguito è riportato un esempio con gli attributi:</span><span class="sxs-lookup"><span data-stu-id="f76c7-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="f76c7-116">Per restituire un cookie al server, il client include un'intestazione cookie nelle richieste successive.</span><span class="sxs-lookup"><span data-stu-id="f76c7-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="f76c7-117">Una risposta HTTP può includere più intestazioni set-cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="f76c7-118">Il client restituisce più cookie utilizzando una singola intestazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="f76c7-119">L'ambito e la durata di un cookie sono controllati dagli attributi seguenti nell'intestazione set-cookie:</span><span class="sxs-lookup"><span data-stu-id="f76c7-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="f76c7-120">**Dominio**: indica al client quale dominio deve ricevere il cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="f76c7-121">Se ad esempio il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com.</span><span class="sxs-lookup"><span data-stu-id="f76c7-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="f76c7-122">Se non è specificato, il dominio è il server di origine.</span><span class="sxs-lookup"><span data-stu-id="f76c7-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="f76c7-123">**Path**: limita il cookie al percorso specificato all'interno del dominio.</span><span class="sxs-lookup"><span data-stu-id="f76c7-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="f76c7-124">Se non specificato, viene usato il percorso dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f76c7-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="f76c7-125">**Expires**: imposta una data di scadenza per il cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="f76c7-126">Il client elimina il cookie quando scade.</span><span class="sxs-lookup"><span data-stu-id="f76c7-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="f76c7-127">**Max-Age**: imposta la validità massima del cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="f76c7-128">Il client elimina il cookie quando raggiunge la validità massima.</span><span class="sxs-lookup"><span data-stu-id="f76c7-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="f76c7-129">Se vengono impostati sia `Expires` che `Max-Age`, `Max-Age` avrà la precedenza.</span><span class="sxs-lookup"><span data-stu-id="f76c7-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="f76c7-130">Se non è impostato alcun oggetto, il client elimina il cookie al termine della sessione corrente.</span><span class="sxs-lookup"><span data-stu-id="f76c7-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="f76c7-131">Il significato esatto di "Session" è determinato dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="f76c7-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="f76c7-132">Tuttavia, tenere presente che i client possono ignorare i cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="f76c7-133">Ad esempio, un utente potrebbe disabilitare i cookie per motivi di privacy.</span><span class="sxs-lookup"><span data-stu-id="f76c7-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="f76c7-134">I client possono eliminare i cookie prima della scadenza o limitare il numero di cookie archiviati.</span><span class="sxs-lookup"><span data-stu-id="f76c7-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="f76c7-135">Per motivi di privacy, i client spesso rifiutano i cookie di "terze parti", dove il dominio non corrisponde al server di origine.</span><span class="sxs-lookup"><span data-stu-id="f76c7-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="f76c7-136">In breve, il server non deve basarsi sul recupero dei cookie che imposta.</span><span class="sxs-lookup"><span data-stu-id="f76c7-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="f76c7-137">Cookie nell'API Web</span><span class="sxs-lookup"><span data-stu-id="f76c7-137">Cookies in Web API</span></span>

<span data-ttu-id="f76c7-138">Per aggiungere un cookie a una risposta HTTP, creare un'istanza di **CookieHeaderValue** che rappresenta il cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="f76c7-139">Chiamare quindi il metodo di estensione **AddCookies** , definito in **System .NET. http. Classe HttpResponseHeadersExtensions** , per aggiungere il cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="f76c7-140">Ad esempio, il codice seguente aggiunge un cookie in un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="f76c7-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="f76c7-141">Si noti che **AddCookies** accetta una matrice di istanze **CookieHeaderValue** .</span><span class="sxs-lookup"><span data-stu-id="f76c7-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="f76c7-142">Per estrarre i cookie da una richiesta client, chiamare il metodo **GetCookies** :</span><span class="sxs-lookup"><span data-stu-id="f76c7-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="f76c7-143">Un **CookieHeaderValue** contiene una raccolta di istanze di **CookieState** .</span><span class="sxs-lookup"><span data-stu-id="f76c7-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="f76c7-144">Ogni **CookieState** rappresenta un cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="f76c7-145">Usare il metodo Indexer per ottenere un **CookieState** in base al nome, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="f76c7-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="f76c7-146">Dati strutturati dei cookie</span><span class="sxs-lookup"><span data-stu-id="f76c7-146">Structured Cookie Data</span></span>

<span data-ttu-id="f76c7-147">Molti browser limitano il numero di cookie in&#8212;cui verranno archiviati sia il numero totale sia il numero per dominio.</span><span class="sxs-lookup"><span data-stu-id="f76c7-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="f76c7-148">Pertanto, può essere utile inserire i dati strutturati in un singolo cookie, anziché impostare più cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="f76c7-149">RFC 6265 non definisce la struttura dei dati dei cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="f76c7-150">Utilizzando la classe **CookieHeaderValue** , è possibile passare un elenco di coppie nome-valore per i dati dei cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="f76c7-151">Queste coppie nome-valore vengono codificate come dati del form con codifica URL nell'intestazione set-cookie:</span><span class="sxs-lookup"><span data-stu-id="f76c7-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="f76c7-152">Il codice precedente produce l'intestazione set-cookie seguente:</span><span class="sxs-lookup"><span data-stu-id="f76c7-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="f76c7-153">La classe **CookieState** fornisce un metodo di indicizzatore per leggere i valori secondari da un cookie nel messaggio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="f76c7-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="f76c7-154">Esempio: impostare e recuperare cookie in un gestore di messaggi</span><span class="sxs-lookup"><span data-stu-id="f76c7-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="f76c7-155">Negli esempi precedenti è stato illustrato come usare i cookie in un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="f76c7-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="f76c7-156">Un'altra opzione consiste nell'utilizzare i [gestori di messaggi](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="f76c7-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="f76c7-157">I gestori di messaggi vengono richiamati in precedenza nella pipeline rispetto ai controller.</span><span class="sxs-lookup"><span data-stu-id="f76c7-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="f76c7-158">Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller oppure aggiungere cookie alla risposta dopo che il controller ha generato la risposta.</span><span class="sxs-lookup"><span data-stu-id="f76c7-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="f76c7-159">Nel codice seguente viene illustrato un gestore di messaggi per la creazione di ID di sessione.</span><span class="sxs-lookup"><span data-stu-id="f76c7-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="f76c7-160">L'ID sessione viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="f76c7-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="f76c7-161">Il gestore controlla la richiesta per il cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="f76c7-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="f76c7-162">Se la richiesta non include il cookie, il gestore genera un nuovo ID di sessione.</span><span class="sxs-lookup"><span data-stu-id="f76c7-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="f76c7-163">In entrambi i casi, il gestore archivia l'ID sessione nel contenitore delle proprietà **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="f76c7-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="f76c7-164">Aggiunge anche il cookie di sessione alla risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f76c7-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="f76c7-165">Questa implementazione non verifica che l'ID di sessione del client sia stato effettivamente emesso dal server.</span><span class="sxs-lookup"><span data-stu-id="f76c7-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="f76c7-166">Non usarlo come una forma di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f76c7-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="f76c7-167">Il punto dell'esempio è mostrare la gestione dei cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="f76c7-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="f76c7-168">Un controller può ottenere l'ID sessione dal contenitore delle proprietà **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="f76c7-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
