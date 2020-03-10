---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usare AJAX per distribuire aggiornamenti dinamici | Microsoft Docs
author: microsoft
description: Il passaggio 10 implementa il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena, usando un approccio basato su AJAX integrato nei dettagli della cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600849"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="65b06-103">Usare AJAX per distribuire aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="65b06-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="65b06-104">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65b06-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="65b06-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="65b06-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="65b06-106">Questo è il passaggio 10 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="65b06-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="65b06-107">Il passaggio 10 implementa il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena, usando un approccio basato su AJAX integrato nella pagina dei dettagli della cena.</span><span class="sxs-lookup"><span data-stu-id="65b06-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="65b06-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="65b06-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="65b06-109">NerdDinner passaggio 10: abilitazione di RSVP accepts</span><span class="sxs-lookup"><span data-stu-id="65b06-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="65b06-110">A questo punto, è possibile implementare il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena.</span><span class="sxs-lookup"><span data-stu-id="65b06-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="65b06-111">Questa operazione verrà abilitata usando un approccio basato su AJAX integrato nella pagina dei dettagli della cena.</span><span class="sxs-lookup"><span data-stu-id="65b06-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="65b06-112">Che indica se l'utente è RSVP</span><span class="sxs-lookup"><span data-stu-id="65b06-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="65b06-113">Gli utenti possono visitare l'URL */dinners/details/[ID*] per visualizzare i dettagli relativi a una cena specifica:</span><span class="sxs-lookup"><span data-stu-id="65b06-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="65b06-114">Il metodo di azione Details () viene implementato in modo analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="65b06-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="65b06-115">Il primo passaggio per implementare il supporto per RSVP consiste nell'aggiungere un metodo di supporto "IsUserRegistered (username)" all'oggetto Dinner (all'interno della classe parziale Dinner.cs creata in precedenza).</span><span class="sxs-lookup"><span data-stu-id="65b06-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="65b06-116">Questo metodo helper restituisce true o false a seconda che l'utente sia attualmente RSVP per la cena:</span><span class="sxs-lookup"><span data-stu-id="65b06-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="65b06-117">È quindi possibile aggiungere il codice seguente al modello di vista Details. aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:</span><span class="sxs-lookup"><span data-stu-id="65b06-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="65b06-118">Ora, quando un utente visita la cena, viene registrato per visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="65b06-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="65b06-119">Quando visitano una cena, non sono registrati per visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="65b06-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="65b06-120">Implementazione del metodo di azione Register</span><span class="sxs-lookup"><span data-stu-id="65b06-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="65b06-121">A questo punto, aggiungere la funzionalità necessaria per consentire agli utenti di rispondere a una cena dalla pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="65b06-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="65b06-122">Per implementare questa operazione, verrà creata una nuova classe "RSVPController" facendo clic con il pulsante destro del mouse sulla directory \Controllers e scegliendo il comando di menu Aggiungi&gt;controller.</span><span class="sxs-lookup"><span data-stu-id="65b06-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="65b06-123">Verrà implementato un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un ID per Dinner come argomento, recupera l'oggetto Dinner appropriato, verifica se l'utente connesso è attualmente nell'elenco degli utenti che hanno eseguito la registrazione e se non aggiunge un oggetto RSVP:</span><span class="sxs-lookup"><span data-stu-id="65b06-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="65b06-124">Si noti sopra come viene restituita una stringa semplice come output del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="65b06-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="65b06-125">È possibile che questo messaggio sia stato incorporato in un modello di vista, ma poiché è talmente piccolo, si userà semplicemente il metodo helper content () nella classe di base controller e viene restituito un messaggio stringa come sopra.</span><span class="sxs-lookup"><span data-stu-id="65b06-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="65b06-126">Chiamata del metodo di azione RSVPForEvent tramite AJAX</span><span class="sxs-lookup"><span data-stu-id="65b06-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="65b06-127">Si userà AJAX per richiamare il metodo di azione Register dalla visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="65b06-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="65b06-128">L'implementazione di questa operazione è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="65b06-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="65b06-129">Verranno innanzitutto aggiunti due riferimenti alla libreria di script:</span><span class="sxs-lookup"><span data-stu-id="65b06-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="65b06-130">La prima libreria fa riferimento alla libreria di script del lato client ASP.NET AJAX di base.</span><span class="sxs-lookup"><span data-stu-id="65b06-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="65b06-131">Questo file ha una dimensione di circa 24K (compresso) e contiene la funzionalità AJAX di base sul lato client.</span><span class="sxs-lookup"><span data-stu-id="65b06-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="65b06-132">La seconda libreria contiene funzioni di utilità che si integrano con i metodi di supporto AJAX incorporati di ASP.NET MVC, che verranno usati a breve.</span><span class="sxs-lookup"><span data-stu-id="65b06-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="65b06-133">È quindi possibile aggiornare il codice del modello di visualizzazione aggiunto in precedenza in modo che, invece di inviare un messaggio "non è registrato per questo evento", viene invece eseguito il rendering di un collegamento che, quando viene eseguito il push, esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent sul controller RSVP e RSVP l'utente:</span><span class="sxs-lookup"><span data-stu-id="65b06-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="65b06-134">Il metodo helper Ajax. ActionLink () usato in precedenza è incorporato in ASP.NET MVC ed è simile al metodo helper HTML. ActionLink () con la differenza che anziché eseguire una navigazione standard esegue una chiamata AJAX al metodo di azione quando si fa clic sul collegamento.</span><span class="sxs-lookup"><span data-stu-id="65b06-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="65b06-135">Sopra viene chiamato il metodo di azione "Register" sul controller "RSVP" e viene passato il DinnerID come parametro "ID".</span><span class="sxs-lookup"><span data-stu-id="65b06-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="65b06-136">Il parametro AjaxOptions finale che si sta passando indica che si desidera ottenere il contenuto restituito dal metodo di azione e aggiornare l'elemento HTML &lt;div&gt; nella pagina il cui ID è "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="65b06-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="65b06-137">A questo punto, quando un utente passa a una cena che non è ancora registrato, visualizzerà un collegamento a RSVP:</span><span class="sxs-lookup"><span data-stu-id="65b06-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="65b06-138">Se si fa clic sul collegamento "RSVP per questo evento", effettuerà una chiamata AJAX al metodo di azione Register sul controller RSVP e al termine verrà visualizzato un messaggio aggiornato come il seguente:</span><span class="sxs-lookup"><span data-stu-id="65b06-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="65b06-139">La larghezza di banda e il traffico di rete interessati durante l'esecuzione di questa chiamata AJAX sono molto leggeri.</span><span class="sxs-lookup"><span data-stu-id="65b06-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="65b06-140">Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene apportata una piccola richiesta HTTP POST Network all'URL */dinners/Register/1* , simile a quanto riportato di seguito in transito:</span><span class="sxs-lookup"><span data-stu-id="65b06-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="65b06-141">E la risposta dal metodo di azione Register è semplicemente:</span><span class="sxs-lookup"><span data-stu-id="65b06-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="65b06-142">Questa chiamata semplice è veloce e funzionerà anche su una rete lenta.</span><span class="sxs-lookup"><span data-stu-id="65b06-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="65b06-143">Aggiunta di un'animazione jQuery</span><span class="sxs-lookup"><span data-stu-id="65b06-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="65b06-144">La funzionalità AJAX implementata è molto efficace e rapida.</span><span class="sxs-lookup"><span data-stu-id="65b06-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="65b06-145">In alcuni casi, tuttavia, un utente potrebbe non notare che il collegamento RSVP è stato sostituito con il nuovo testo.</span><span class="sxs-lookup"><span data-stu-id="65b06-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="65b06-146">Per rendere più ovvio il risultato, è possibile aggiungere un'animazione semplice per attirare l'attenzione sul messaggio di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="65b06-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="65b06-147">Il modello di progetto MVC ASP.NET predefinito include jQuery, una libreria JavaScript open source eccellente e molto diffusa, supportata anche da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="65b06-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="65b06-148">jQuery offre una serie di funzionalità, tra cui una libreria di selezione di DOM HTML ed effetti.</span><span class="sxs-lookup"><span data-stu-id="65b06-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="65b06-149">Per usare jQuery, aggiungeremo prima di tutto un riferimento a script.</span><span class="sxs-lookup"><span data-stu-id="65b06-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="65b06-150">Poiché si userà jQuery in una vasta gamma di posizioni all'interno del sito, si aggiungerà il riferimento allo script nel file della pagina master del sito. Master in modo che tutte le pagine possano usarlo.</span><span class="sxs-lookup"><span data-stu-id="65b06-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="65b06-151">*Suggerimento: assicurarsi di aver installato l'hotfix JavaScript IntelliSense per VS 2008 SP1 che consente il supporto IntelliSense più completo per i file JavaScript (incluso jQuery). È possibile scaricarlo da: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="65b06-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="65b06-152">Il codice scritto utilizzando JQuery spesso utilizza un metodo JavaScript globale "$ ()" che recupera uno o più elementi HTML utilizzando un selettore CSS.</span><span class="sxs-lookup"><span data-stu-id="65b06-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="65b06-153">Ad esempio, *$ ("#rsvpmsg")* seleziona qualsiasi elemento HTML con ID rsvpmsg, mentre *$ (". something")* seleziona tutti gli elementi con il nome della classe CSS "Something".</span><span class="sxs-lookup"><span data-stu-id="65b06-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="65b06-154">È anche possibile scrivere query più avanzate, ad esempio "Restituisci tutti i pulsanti di opzione controllati" usando una query di selezione come: *$ ("input [@type= Radio] [@checked]")* .</span><span class="sxs-lookup"><span data-stu-id="65b06-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="65b06-155">Dopo aver selezionato gli elementi, è possibile chiamare i metodi su di essi per eseguire un'azione, ad esempio per nasconderli: *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="65b06-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="65b06-156">Per lo scenario RSVP, definiamo una semplice funzione JavaScript denominata "AnimateRSVPMessage" che seleziona "rsvpmsg" &lt;div&gt; e aggiunge un'animazione alla dimensione del contenuto di testo.</span><span class="sxs-lookup"><span data-stu-id="65b06-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="65b06-157">Il codice riportato di seguito avvia il testo di dimensioni ridotte e quindi ne causa l'aumento in un intervallo di tempo di 400 millisecondi:</span><span class="sxs-lookup"><span data-stu-id="65b06-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="65b06-158">È quindi possibile collegare questa funzione JavaScript affinché venga chiamata dopo che la chiamata AJAX è stata completata correttamente passando il nome al metodo helper Ajax. ActionLink () (tramite la proprietà di evento AjaxOptions "onSuccess"):</span><span class="sxs-lookup"><span data-stu-id="65b06-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="65b06-159">A questo punto, quando si fa clic sul collegamento "RSVP per questo evento" e la chiamata AJAX viene completata correttamente, il messaggio di contenuto restituito verrà animato e aumenterà di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="65b06-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="65b06-160">Oltre a fornire un evento "onSuccess", l'oggetto AjaxOptions espone gli eventi OnBegin, OnFailure e OnComplete che è possibile gestire, insieme a un'ampia gamma di altre proprietà e opzioni utili.</span><span class="sxs-lookup"><span data-stu-id="65b06-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="65b06-161">Pulizia: effettuare il refactoring di una visualizzazione parziale RSVP</span><span class="sxs-lookup"><span data-stu-id="65b06-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="65b06-162">Il modello di visualizzazione dei dettagli sta iniziando a essere un po' più difficile da comprendere.</span><span class="sxs-lookup"><span data-stu-id="65b06-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="65b06-163">Per contribuire a migliorare la leggibilità del codice, è necessario creare una visualizzazione parziale, RSVPStatus. ascx, che incapsula tutto il codice di visualizzazione RSVP per la pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="65b06-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="65b06-164">Questa operazione può essere eseguita facendo clic con il pulsante destro del mouse sulla cartella \Views\Dinners e scegliendo il comando di menu Aggiungi&gt;visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="65b06-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="65b06-165">Si avrà un oggetto Dinner come ViewModel fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="65b06-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="65b06-166">È quindi possibile copiare e incollare il contenuto RSVP dalla vista Details. aspx al suo interno.</span><span class="sxs-lookup"><span data-stu-id="65b06-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="65b06-167">Una volta completata questa operazione, viene creata anche un'altra visualizzazione parziale, EditAndDeleteLinks. ascx, che incapsula il codice di visualizzazione collegamento modifica ed Elimina.</span><span class="sxs-lookup"><span data-stu-id="65b06-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="65b06-168">In questo modo, si avrà a disposizione un oggetto Dinner come ViewModel fortemente tipizzato, che verrà copiato e incollato dalla visualizzazione dettagli. aspx.</span><span class="sxs-lookup"><span data-stu-id="65b06-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="65b06-169">Il modello di visualizzazione dettagli può includere solo due chiamate al metodo HTML. RenderPartial () nella parte inferiore:</span><span class="sxs-lookup"><span data-stu-id="65b06-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="65b06-170">In questo modo il codice pulisce la lettura e la gestione.</span><span class="sxs-lookup"><span data-stu-id="65b06-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="65b06-171">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="65b06-171">Next Step</span></span>

<span data-ttu-id="65b06-172">Esaminiamo ora come è possibile usare ulteriormente AJAX e aggiungere il supporto per il mapping interattivo all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65b06-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="65b06-173">[Precedente](secure-applications-using-authentication-and-authorization.md)
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="65b06-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
