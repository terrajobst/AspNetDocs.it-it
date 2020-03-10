---
uid: whitepapers/request-validation
title: Convalida delle richieste-prevenzione degli attacchi script | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la funzionalità di convalida della richiesta di ASP.NET, in cui, per impostazione predefinita, l'applicazione non è in grado di elaborare il contenuto HTML non codificato inviato...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640847"
---
# <a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="ff546-103">Richiedere la convalida - Prevenzione degli attacchi tramite script</span><span class="sxs-lookup"><span data-stu-id="ff546-103">Request Validation - Preventing Script Attacks</span></span>

> <span data-ttu-id="ff546-104">In questo documento viene descritta la funzionalità di convalida della richiesta di ASP.NET, in cui, per impostazione predefinita, l'applicazione non è in grado di elaborare contenuto HTML non codificato inviato al server.</span><span class="sxs-lookup"><span data-stu-id="ff546-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="ff546-105">Questa funzionalità di convalida della richiesta può essere disabilitata quando l'applicazione è stata progettata per elaborare in modo sicuro i dati HTML.</span><span class="sxs-lookup"><span data-stu-id="ff546-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="ff546-106">Si applica a ASP.NET 1,1 e ASP.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="ff546-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>

<span data-ttu-id="ff546-107">La convalida della richiesta, una funzionalità di ASP.NET sin dalla versione 1.1, impedisce al server di accettare contenuti che includono HTML non codificato.</span><span class="sxs-lookup"><span data-stu-id="ff546-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="ff546-108">Questa funzionalità è progettata per impedire alcuni attacchi script injection in cui il codice script client o HTML può essere inconsapevolmente inviato a un server, archiviato e quindi presentato ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="ff546-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="ff546-109">È tuttavia vivamente consigliabile convalidare tutti i dati di input e codificarli con HTML, se appropriato.</span><span class="sxs-lookup"><span data-stu-id="ff546-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="ff546-110">Si crea, ad esempio, una pagina Web che richiede l'indirizzo di posta elettronica di un utente e quindi archivia tale indirizzo di posta elettronica in un database.</span><span class="sxs-lookup"><span data-stu-id="ff546-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="ff546-111">Se l'utente immette &lt;SCRIPT&gt;avviso ("Hello from script")&lt;/SCRIPT&gt; anziché un indirizzo di posta elettronica valido, quando i dati vengono presentati, questo script può essere eseguito se il contenuto non è stato codificato correttamente.</span><span class="sxs-lookup"><span data-stu-id="ff546-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="ff546-112">La funzionalità di convalida della richiesta di ASP.NET impedisce che ciò accada.</span><span class="sxs-lookup"><span data-stu-id="ff546-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="ff546-113">Perché questa funzionalità è utile</span><span class="sxs-lookup"><span data-stu-id="ff546-113">Why this feature is useful</span></span>

<span data-ttu-id="ff546-114">Molti siti non sono consapevoli del fatto che sono aperti a attacchi di script injection semplici.</span><span class="sxs-lookup"><span data-stu-id="ff546-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="ff546-115">Indipendentemente dal fatto che lo scopo di questi attacchi sia quello di rivolto al sito visualizzando il codice HTML o di eseguire potenzialmente uno script client per reindirizzare l'utente al sito di un pirata informatico, gli attacchi di script injection costituiscono un problema che gli sviluppatori Web devono affrontare.</span><span class="sxs-lookup"><span data-stu-id="ff546-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="ff546-116">Gli attacchi di script injection rappresentano un problema per tutti gli sviluppatori Web, che usano ASP.NET, ASP o altre tecnologie di sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="ff546-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="ff546-117">La funzionalità di convalida della richiesta ASP.NET impedisce in modo proattivo questi attacchi evitando che il contenuto HTML non codificato venga elaborato dal server, a meno che lo sviluppatore non decida di consentire tale contenuto.</span><span class="sxs-lookup"><span data-stu-id="ff546-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="ff546-118">Cosa aspettarsi: pagina di errore</span><span class="sxs-lookup"><span data-stu-id="ff546-118">What to expect: Error Page</span></span>

<span data-ttu-id="ff546-119">Lo screenshot seguente mostra alcuni esempi di codice ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="ff546-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="ff546-120">Eseguendo questo codice si ottiene una pagina semplice che consente di immettere testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:</span><span class="sxs-lookup"><span data-stu-id="ff546-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="ff546-121">Tuttavia, era JavaScript, ad esempio `<script>alert("hello!")</script>` da immettere e inviare, si otterrebbe un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="ff546-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="ff546-122">Il messaggio di errore indica che è stato rilevato un valore ' request. Form potenzialmente pericoloso ', che fornisce ulteriori dettagli nella descrizione di quanto si è verificato e come modificare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="ff546-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="ff546-123">Esempio:</span><span class="sxs-lookup"><span data-stu-id="ff546-123">For example:</span></span>

<span data-ttu-id="ff546-124">La convalida della richiesta ha rilevato un valore di input client potenzialmente pericoloso e l'elaborazione della richiesta è stata interrotta.</span><span class="sxs-lookup"><span data-stu-id="ff546-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="ff546-125">Questo valore può indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di scripting tra siti.</span><span class="sxs-lookup"><span data-stu-id="ff546-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="ff546-126">È possibile disabilitare la convalida delle richieste impostando `validateRequest=false` nella direttiva della pagina o nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ff546-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="ff546-127">Tuttavia, si consiglia vivamente all'applicazione di controllare in modo esplicito tutti gli input in questo caso.</span><span class="sxs-lookup"><span data-stu-id="ff546-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="ff546-128">Disabilitazione della convalida delle richieste in una pagina</span><span class="sxs-lookup"><span data-stu-id="ff546-128">Disabling request validation on a page</span></span>

<span data-ttu-id="ff546-129">Per disabilitare la convalida delle richieste in una pagina, è necessario impostare l'attributo `validateRequest` della direttiva della pagina su `false`:</span><span class="sxs-lookup"><span data-stu-id="ff546-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="ff546-130">Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato a una pagina; è responsabilità dello sviluppatore della pagina garantire che il contenuto sia codificato o elaborato correttamente.</span><span class="sxs-lookup"><span data-stu-id="ff546-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="ff546-131">Disabilitazione della convalida delle richieste per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="ff546-131">Disabling request validation for your application</span></span>

<span data-ttu-id="ff546-132">Per disabilitare la convalida delle richieste per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest della sezione `<pages />` su `false`:</span><span class="sxs-lookup"><span data-stu-id="ff546-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="ff546-133">Se si desidera disabilitare la convalida delle richieste per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="ff546-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="ff546-134">Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato all'applicazione; è responsabilità dello sviluppatore dell'applicazione garantire che il contenuto sia codificato o elaborato correttamente.</span><span class="sxs-lookup"><span data-stu-id="ff546-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="ff546-135">Il codice seguente è stato modificato per disattivare la convalida delle richieste:</span><span class="sxs-lookup"><span data-stu-id="ff546-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="ff546-136">A questo punto, se il codice JavaScript seguente è stato immesso nella casella di testo `<script>alert("hello!")</script>` il risultato sarà:</span><span class="sxs-lookup"><span data-stu-id="ff546-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="ff546-137">Per evitare che ciò accada, quando la convalida delle richieste è disattivata, è necessario codificare in HTML il contenuto.</span><span class="sxs-lookup"><span data-stu-id="ff546-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="ff546-138">Come codificare il contenuto HTML</span><span class="sxs-lookup"><span data-stu-id="ff546-138">How to HTML encode content</span></span>

<span data-ttu-id="ff546-139">Se è stata disabilitata la convalida delle richieste, è consigliabile codificare il contenuto HTML che verrà archiviato per un uso futuro.</span><span class="sxs-lookup"><span data-stu-id="ff546-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="ff546-140">La codifica HTML sostituirà automaticamente qualsiasi elemento '&lt;' o '&gt;' (insieme a diversi altri simboli) con la relativa rappresentazione codificata HTML corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ff546-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="ff546-141">Ad esempio,'&lt;' viene sostituito da'&amp;lt;' è&gt;' viene sostituito da'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="ff546-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="ff546-142">I browser usano questi codici speciali per visualizzare '&lt;' o '&gt;' nel browser.</span><span class="sxs-lookup"><span data-stu-id="ff546-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="ff546-143">Il contenuto può essere facilmente codificato in HTML nel server usando l'API `Server.HtmlEncode(string)`.</span><span class="sxs-lookup"><span data-stu-id="ff546-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="ff546-144">Il contenuto può anche essere facilmente decodificato in formato HTML, ovvero ripristinato in HTML standard usando il metodo `Server.HtmlDecode(string)`.</span><span class="sxs-lookup"><span data-stu-id="ff546-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="ff546-145">Risultato:</span><span class="sxs-lookup"><span data-stu-id="ff546-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
