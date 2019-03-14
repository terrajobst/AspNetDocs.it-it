---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: L'invio di posta elettronica da un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: Rick-Anderson
description: In questo capitolo illustra come inviare un messaggio di posta elettronica automatizzati da un sito Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: f388ac1a97bde39cffe6b592b436d7af0d419a5f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038398"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d2d33-103">L'invio di posta elettronica da un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="d2d33-103">Sending Email from an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d2d33-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d2d33-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d2d33-105">Questo articolo illustra come inviare un messaggio di posta elettronica da un sito Web quando si usa ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="d2d33-105">This article explains how to send an email message from a website when you use ASP.NET Web Pages (Razor).</span></span>
> 
> <span data-ttu-id="d2d33-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="d2d33-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d2d33-107">Come inviare un messaggio di posta elettronica tramite il sito Web.</span><span class="sxs-lookup"><span data-stu-id="d2d33-107">How to send an email message from your website.</span></span>
> - <span data-ttu-id="d2d33-108">Come collegare un file a un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-108">How to attach a file to an email message.</span></span>
> 
> <span data-ttu-id="d2d33-109">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="d2d33-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="d2d33-110">Il `WebMail` helper.</span><span class="sxs-lookup"><span data-stu-id="d2d33-110">The `WebMail` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d2d33-111">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d2d33-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d2d33-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d2d33-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d2d33-113">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d2d33-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a><span data-ttu-id="d2d33-114">L'invio di messaggi di posta elettronica tramite il sito Web</span><span class="sxs-lookup"><span data-stu-id="d2d33-114">Sending Email Messages from Your Website</span></span>

<span data-ttu-id="d2d33-115">Esistono molti motivi per cui potrebbe essere necessario inviare posta elettronica tramite il sito Web.</span><span class="sxs-lookup"><span data-stu-id="d2d33-115">There are all sorts of reasons why you might need to send email from your website.</span></span> <span data-ttu-id="d2d33-116">È possibile inviare i messaggi di conferma per gli utenti oppure è possibile inviare notifiche a se stessi (ad esempio, che ha registrato un nuovo utente). Il `WebMail` helper rende più semplice per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-116">You might send confirmation messages to users, or you might send notifications to yourself (for example, that a new user has registered.) The `WebMail` helper makes it easy for you to send email.</span></span>

<span data-ttu-id="d2d33-117">Usare il `WebMail` helper, è necessario avere accesso a un server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-117">To use the `WebMail` helper, you have to have access to an SMTP server.</span></span> <span data-ttu-id="d2d33-118">(SMTP è l'acronimo *Simple Mail Transfer Protocol*.) Un server SMTP è un server di posta elettronica che inoltra solo messaggi al server del destinatario &#8212; è il lato in uscita del messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-118">(SMTP stands for *Simple Mail Transfer Protocol*.) An SMTP server is an email server that only forwards messages to the recipient's server &#8212; it's the outbound side of email.</span></span> <span data-ttu-id="d2d33-119">Se si usa un provider di hosting per il sito Web, probabilmente procedere alla configurazione con la posta elettronica e in modo da capire che cos'è il nome del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-119">If you use a hosting provider for your website, they probably set you up with email and they can tell you what your SMTP server name is.</span></span> <span data-ttu-id="d2d33-120">Se si lavora all'interno di una rete aziendale, un amministratore o il reparto IT può in genere fornire le informazioni su un server SMTP che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="d2d33-120">If you're working inside a corporate network, an administrator or your IT department can usually give you the information about an SMTP server that you can use.</span></span> <span data-ttu-id="d2d33-121">Se si lavora da casa, è anche possibile eseguire la verifica utilizzando il provider di posta elettronica normale, che è possibile indicare il nome del proprio server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-121">If you're working at home, you might even be able to test using your ordinary email provider, who can tell you the name of their SMTP server.</span></span> <span data-ttu-id="d2d33-122">È in genere necessario:</span><span class="sxs-lookup"><span data-stu-id="d2d33-122">You typically need:</span></span>

- <span data-ttu-id="d2d33-123">Il nome del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-123">The name of the SMTP server.</span></span>
- <span data-ttu-id="d2d33-124">Il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="d2d33-124">The port number.</span></span> <span data-ttu-id="d2d33-125">Si tratta quasi sempre di 25.</span><span class="sxs-lookup"><span data-stu-id="d2d33-125">This is almost always 25.</span></span> <span data-ttu-id="d2d33-126">Tuttavia, potrebbe essere necessario usare la porta 587 ISP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-126">However, your ISP may require you to use port 587.</span></span> <span data-ttu-id="d2d33-127">Se si usa (SSL) secure sockets layer per la posta elettronica, si potrebbe essere una porta diversa.</span><span class="sxs-lookup"><span data-stu-id="d2d33-127">If you are using secure sockets layer (SSL) for email, you might need a different port.</span></span> <span data-ttu-id="d2d33-128">Rivolgersi al provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-128">Check with your email provider.</span></span>
- <span data-ttu-id="d2d33-129">Credenziali (nome utente, password).</span><span class="sxs-lookup"><span data-stu-id="d2d33-129">Credentials (user name, password).</span></span>

<span data-ttu-id="d2d33-130">In questa procedura, creare due pagine.</span><span class="sxs-lookup"><span data-stu-id="d2d33-130">In this procedure, you create two pages.</span></span> <span data-ttu-id="d2d33-131">La prima pagina ha un formato che consente agli utenti di immettere una descrizione, come se essi sono stati compilando un modulo di supporto tecnico.</span><span class="sxs-lookup"><span data-stu-id="d2d33-131">The first page has a form that lets users enter a description, as if they were filling in a technical-support form.</span></span> <span data-ttu-id="d2d33-132">La prima pagina invia le informazioni a un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="d2d33-132">The first page submits its information to a second page.</span></span> <span data-ttu-id="d2d33-133">Nella seconda pagina, codice estrae le informazioni dell'utente e invia un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-133">In the second page, code extracts the user's information and sends an email message.</span></span> <span data-ttu-id="d2d33-134">Visualizza inoltre un messaggio di conferma che è stata ricevuta la segnalazione del problema.</span><span class="sxs-lookup"><span data-stu-id="d2d33-134">It also displays a message confirming that the problem report has been received.</span></span>

![[immagine]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> <span data-ttu-id="d2d33-136">Per semplificare questo esempio, il codice inizializza il `WebMail` destra helper della pagina in cui viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="d2d33-136">To keep this example simple, the code initializes the `WebMail` helper right in the page where you use it.</span></span> <span data-ttu-id="d2d33-137">Tuttavia, per i siti Web reali, è preferibile inserire codice di inizializzazione simile al seguente in un file globale, in modo che si inizializza il `WebMail` helper per tutti i file nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="d2d33-137">However, for real websites, it's a better idea to put initialization code like this in a global file, so that you initialize the `WebMail` helper for all files in your website.</span></span> <span data-ttu-id="d2d33-138">Per altre informazioni, vedere [personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).</span><span class="sxs-lookup"><span data-stu-id="d2d33-138">For more information, see [Customizing Site-Wide Behavior for ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).</span></span>


1. <span data-ttu-id="d2d33-139">Creare un nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="d2d33-139">Create a new website.</span></span>
2. <span data-ttu-id="d2d33-140">Aggiungere una nuova pagina denominata *EmailRequest.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d2d33-140">Add a new page named *EmailRequest.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    <span data-ttu-id="d2d33-141">Si noti che il `action` attributo dell'elemento del form è stato impostato su *ProcessRequest.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d2d33-141">Notice that the `action` attribute of the form element has been set to *ProcessRequest.cshtml*.</span></span> <span data-ttu-id="d2d33-142">Ciò significa che il modulo verrà inviato a tale pagina anziché indietro alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="d2d33-142">This means that the form will be submitted to that page instead of back to the current page.</span></span>
3. <span data-ttu-id="d2d33-143">Aggiungere una nuova pagina denominata *ProcessRequest.cshtml* al sito Web e aggiungere il codice e il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d2d33-143">Add a new page named *ProcessRequest.cshtml* to the website and add the following code and markup:</span></span>   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    <span data-ttu-id="d2d33-144">Nel codice, è ottenere i valori dei campi modulo che sono stati inviati alla pagina.</span><span class="sxs-lookup"><span data-stu-id="d2d33-144">In the code, you get the values of the form fields that were submitted to the page.</span></span> <span data-ttu-id="d2d33-145">È quindi possibile chiamare il `WebMail` dell'helper `Send` metodo per creare e inviare il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-145">You then call the `WebMail` helper's `Send` method to create and send the email message.</span></span> <span data-ttu-id="d2d33-146">In questo caso, i valori da utilizzare sono costituiti da testo che si concatena con i valori che sono stati inviati dal modulo.</span><span class="sxs-lookup"><span data-stu-id="d2d33-146">In this case, the values to use are made up of text that you concatenate with the values that were submitted from the form.</span></span>

    <span data-ttu-id="d2d33-147">Il codice per questa pagina è all'interno di un `try/catch` blocco.</span><span class="sxs-lookup"><span data-stu-id="d2d33-147">The code for this page is inside a `try/catch` block.</span></span> <span data-ttu-id="d2d33-148">Se per qualsiasi motivo il tentativo di inviare un messaggio di posta elettronica non funziona correttamente (ad esempio, le impostazioni non sono a destra), il codice nel `catch` blocco viene eseguito e imposta il `errorMessage` variabile all'errore che si è verificato.</span><span class="sxs-lookup"><span data-stu-id="d2d33-148">If for any reason the attempt to send an email doesn't work (for example, the settings aren't right), the code in the `catch` block runs and sets the `errorMessage` variable to the error that has occurred.</span></span> <span data-ttu-id="d2d33-149">(Per altre informazioni sulle `try/catch` blocchi o le `<text>` tag, vedere [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)</span><span class="sxs-lookup"><span data-stu-id="d2d33-149">(For more information about `try/catch` blocks or the `<text>` tag, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)</span></span>

    <span data-ttu-id="d2d33-150">Nel corpo della pagina, se il `errorMessage` variabile è vuota (impostazione predefinita), l'utente visualizza un messaggio che è stato inviato il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-150">In the body of the page, if the `errorMessage` variable is empty (the default), the user sees a message that the email message has been sent.</span></span> <span data-ttu-id="d2d33-151">Se il `errorMessage` variabile è impostata su true, l'utente vede un messaggio che si è verificato un problema durante l'invio del messaggio.</span><span class="sxs-lookup"><span data-stu-id="d2d33-151">If the `errorMessage` variable is set to true, the user sees a message that there's been a problem sending the message.</span></span>

    <span data-ttu-id="d2d33-152">Si noti che nella parte della pagina che visualizza un messaggio di errore, non esiste un test aggiuntivi: `if(debuggingFlag)`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-152">Notice that in the portion of the page that displays an error message, there's an additional test: `if(debuggingFlag)`.</span></span> <span data-ttu-id="d2d33-153">Questa è una variabile che è possibile impostare su true se si verificano problemi durante l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-153">This is a variable that you can set to true if you're having trouble sending email.</span></span> <span data-ttu-id="d2d33-154">Quando si `debuggingFlag` è true, e se è presente un problema durante l'invio di posta elettronica, viene visualizzato un messaggio di errore aggiuntive che mostra tutti i valori ASP.NET ha segnalato durante il tentativo di inviare il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-154">When `debuggingFlag` is true, and if there's a problem sending email, an additional error message is displayed that shows whatever ASP.NET has reported when it tried to send the email message.</span></span> <span data-ttu-id="d2d33-155">Avviso corretto, però: i messaggi di errore che segnala di ASP.NET quando Impossibile inviare un messaggio di posta elettronica possono essere generici.</span><span class="sxs-lookup"><span data-stu-id="d2d33-155">Fair warning, though: the error messages that ASP.NET reports when it can't send an email message can be generic.</span></span> <span data-ttu-id="d2d33-156">Ad esempio, se ASP.NET non riesce a contattare il server SMTP (ad esempio, perché è stato commesso un errore nel nome del server), l'errore è `Failure sending mail`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-156">For example, if ASP.NET can't contact the SMTP server (for example, because you made an error in the server name), the error is `Failure sending mail`.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="d2d33-157">**Importanti** quando viene visualizzato un messaggio di errore da un oggetto eccezione (`ex` nel codice), si *non* regolarmente passare tale messaggio tramite agli utenti.</span><span class="sxs-lookup"><span data-stu-id="d2d33-157">**Important** When you get an error message from an exception object (`ex` in the code), do *not* routinely pass that message through to users.</span></span> <span data-ttu-id="d2d33-158">Oggetti eccezione includono spesso informazioni che gli utenti non devono essere visualizzati e che può essere anche una vulnerabilità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="d2d33-158">Exception objects often include information that users should not see and that can even be a security vulnerability.</span></span> <span data-ttu-id="d2d33-159">Ecco perché questo codice include la variabile `debuggingFlag` che viene usato come un'opzione per visualizzare il messaggio di errore e il motivo per cui la variabile per impostazione predefinita è impostata su false.</span><span class="sxs-lookup"><span data-stu-id="d2d33-159">That's why this code includes the variable `debuggingFlag` that's used as a switch to display the error message, and why the variable by default is set to false.</span></span> <span data-ttu-id="d2d33-160">È consigliabile impostare tale variabile su true (e pertanto visualizzare il messaggio di errore) *solo* se si riscontra un problema con l'invio di posta elettronica ed è necessario eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="d2d33-160">You should set that variable to true (and therefore display the error message) *only* if you're having a problem with sending email and you need to debug.</span></span> <span data-ttu-id="d2d33-161">Dopo aver risolto eventuali problemi, impostare `debuggingFlag` reimpostarla su false.</span><span class="sxs-lookup"><span data-stu-id="d2d33-161">Once you have fixed any problems, set `debuggingFlag` back to false.</span></span>

    <span data-ttu-id="d2d33-162">Modificare le impostazioni correlate nel codice di posta elettronica seguenti:</span><span class="sxs-lookup"><span data-stu-id="d2d33-162">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="d2d33-163">Impostare `your-SMTP-host` sul nome del server SMTP che è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="d2d33-163">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="d2d33-164">Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-164">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="d2d33-165">Impostare `your-account-password` alla password per l'account del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-165">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="d2d33-166">Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-166">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="d2d33-167">Questo è il messaggio viene inviato dall'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-167">This is the email address that the message is sent from.</span></span> <span data-ttu-id="d2d33-168">(Alcuni provider di posta elettronica non consentono di specificare un diverso `From` indirizzi e userà il nome utente come il `From` indirizzo.)</span><span class="sxs-lookup"><span data-stu-id="d2d33-168">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a><span data-ttu-id="d2d33-169">Configurazione delle impostazioni di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d2d33-169">Configuring Email Settings</span></span>
     > 
     > <span data-ttu-id="d2d33-170">Può essere un problema in alcuni casi per assicurarsi di avere le impostazioni corrette per il server SMTP, numero di porta e così via.</span><span class="sxs-lookup"><span data-stu-id="d2d33-170">It can be a challenge sometimes to make sure you have the right settings for the SMTP server, port number, and so on.</span></span> <span data-ttu-id="d2d33-171">Di seguito sono riportati alcuni suggerimenti:</span><span class="sxs-lookup"><span data-stu-id="d2d33-171">Here are a few tips:</span></span>
     > 
     > - <span data-ttu-id="d2d33-172">Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-172">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="d2d33-173">Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a quel punto possibile `localhost`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-173">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="d2d33-174">Questo avviene perché dopo aver pubblicato e il sito è in esecuzione nel server del provider, il server di posta elettronica potrebbe essere locale dal punto di vista dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d2d33-174">This is because after you've published and your site is running on the provider's server, the email server might be local from the perspective of your application.</span></span> <span data-ttu-id="d2d33-175">Questa modifica nei nomi di server potrebbe essere che necessario modificare il nome del server SMTP come parte del processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d2d33-175">This change in server names might mean you have to change the SMTP server name as part of your publishing process.</span></span>
     > - <span data-ttu-id="d2d33-176">Il numero di porta in genere è 25.</span><span class="sxs-lookup"><span data-stu-id="d2d33-176">The port number is usually 25.</span></span> <span data-ttu-id="d2d33-177">Tuttavia, alcuni provider richiedono di usare la porta 587 o alcune porte.</span><span class="sxs-lookup"><span data-stu-id="d2d33-177">However, some providers require you to use port 587 or some other port.</span></span>
     > - <span data-ttu-id="d2d33-178">Assicurarsi di usare le credenziali corrette.</span><span class="sxs-lookup"><span data-stu-id="d2d33-178">Make sure that you use the right credentials.</span></span> <span data-ttu-id="d2d33-179">Se il sito è stata pubblicata in un provider di hosting, usare le credenziali che il provider ha segnalato in modo specifico sono per la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-179">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="d2d33-180">Questi potrebbero essere diversi dalle credenziali che utilizzare per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d2d33-180">These might be different from the credentials you use to publish.</span></span>
     > - <span data-ttu-id="d2d33-181">In alcuni casi non occorre credenziali affatto.</span><span class="sxs-lookup"><span data-stu-id="d2d33-181">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="d2d33-182">Se si sta inviando e-mail tramite all'account personale, il provider di posta elettronica sappia già le proprie credenziali.</span><span class="sxs-lookup"><span data-stu-id="d2d33-182">If you're sending email using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="d2d33-183">Dopo aver pubblicato, è necessario usare credenziali diverse da quelle quando si testa nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="d2d33-183">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
     > - <span data-ttu-id="d2d33-184">Se il provider di posta elettronica utilizza la crittografia, è necessario impostare `WebMail.EnableSsl` a `true`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-184">If your email provider uses encryption, you have to set `WebMail.EnableSsl` to `true`.</span></span>
4. <span data-ttu-id="d2d33-185">Eseguire la *EmailRequest.cshtml* pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="d2d33-185">Run the *EmailRequest.cshtml* page in a browser.</span></span> <span data-ttu-id="d2d33-186">(Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)</span><span class="sxs-lookup"><span data-stu-id="d2d33-186">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
5. <span data-ttu-id="d2d33-187">Immettere il nome e una descrizione del problema e quindi scegliere il **Submit** pulsante.</span><span class="sxs-lookup"><span data-stu-id="d2d33-187">Enter your name and a problem description, and then click the **Submit** button.</span></span> <span data-ttu-id="d2d33-188">Si verrà reindirizzati per il *ProcessRequest.cshtml* pagina per confermare che il messaggio e che invia un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-188">You're redirected to the *ProcessRequest.cshtml* page, which confirms your message and which sends you an email message.</span></span> 

    ![[immagine]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a><span data-ttu-id="d2d33-190">L'invio di un File tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d2d33-190">Sending a File Using Email</span></span>

<span data-ttu-id="d2d33-191">È anche possibile inviare i file collegati per i messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-191">You can also send files that are attached to email messages.</span></span> <span data-ttu-id="d2d33-192">In questa procedura, si crea un file di testo e due pagine HTML.</span><span class="sxs-lookup"><span data-stu-id="d2d33-192">In this procedure, you create a text file and two HTML pages.</span></span> <span data-ttu-id="d2d33-193">Si userà il file di testo come allegato di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-193">You'll use the text file as an email attachment.</span></span>

1. <span data-ttu-id="d2d33-194">Nel sito Web, aggiungere un nuovo file di testo e denominarlo *MyFile*.</span><span class="sxs-lookup"><span data-stu-id="d2d33-194">In the website, add a new text file and name it *MyFile.txt*.</span></span>
2. <span data-ttu-id="d2d33-195">Copiare il testo seguente e incollarlo nel file:</span><span class="sxs-lookup"><span data-stu-id="d2d33-195">Copy the following text and paste it in the file:</span></span> 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. <span data-ttu-id="d2d33-196">Creare una pagina denominata *SendFile.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d2d33-196">Create a page named *SendFile.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. <span data-ttu-id="d2d33-197">Creare una pagina denominata *ProcessFile.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d2d33-197">Create a page named *ProcessFile.cshtml* and add the following markup:</span></span> 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. <span data-ttu-id="d2d33-198">Modificare le impostazioni correlate nel codice dell'esempio di posta elettronica seguenti:</span><span class="sxs-lookup"><span data-stu-id="d2d33-198">Modify the following email related settings in the code from the example:</span></span>

    - <span data-ttu-id="d2d33-199">Impostare `your-SMTP-host` al nome di un server SMTM che è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="d2d33-199">Set `your-SMTP-host` to the name of an SMTP server that you have access to.</span></span>
    - <span data-ttu-id="d2d33-200">Impostare `your-user-name-here` per il nome utente per l'account del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-200">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
    - <span data-ttu-id="d2d33-201">Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-201">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="d2d33-202">Questo è il messaggio viene inviato dall'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-202">This is the email address that the message is sent from.</span></span>
    - <span data-ttu-id="d2d33-203">Impostare `your-account-password` alla password per l'account del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2d33-203">Set `your-account-password` to the password for your SMTP server account.</span></span>
    - <span data-ttu-id="d2d33-204">Impostare `target-email-address-here` per il proprio indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d2d33-204">Set `target-email-address-here` to your own email address.</span></span> <span data-ttu-id="d2d33-205">(Come prima, si sarebbe in genere invia un messaggio di posta elettronica a un altro utente, ma per i test, è possibile inviarlo a se stessi).</span><span class="sxs-lookup"><span data-stu-id="d2d33-205">(As before, you'd normally send an email to someone else, but for testing, you can send it to yourself.)</span></span>
6. <span data-ttu-id="d2d33-206">Eseguire la *SendFile.cshtml* pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="d2d33-206">Run the *SendFile.cshtml* page in a browser.</span></span>
7. <span data-ttu-id="d2d33-207">Immettere il nome del file di testo per collegare il proprio nome e una riga dell'oggetto (*MyFile*).</span><span class="sxs-lookup"><span data-stu-id="d2d33-207">Enter your name, a subject line, and the name of the text file to attach (*MyFile.txt*).</span></span>
8. <span data-ttu-id="d2d33-208">Fare clic sul pulsante `Submit`.</span><span class="sxs-lookup"><span data-stu-id="d2d33-208">Click the `Submit` button.</span></span> <span data-ttu-id="d2d33-209">Come in precedenza, si verrà reindirizzati per il *ProcessFile.cshtml* pagina per confermare che il messaggio e che invia un messaggio di posta elettronica con il file allegato.</span><span class="sxs-lookup"><span data-stu-id="d2d33-209">As before, you're redirected to the *ProcessFile.cshtml* page, which confirms your message and which sends you an email message with the attached file.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d2d33-210">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d2d33-210">Additional Resources</span></span>


- [<span data-ttu-id="d2d33-211">Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d2d33-211">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)
- [<span data-ttu-id="d2d33-212">Simple Mail Transfer Protocol</span><span class="sxs-lookup"><span data-stu-id="d2d33-212">Simple Mail Transfer Protocol</span></span>](https://msdn.microsoft.com/library/aa480435.aspx)
- [<span data-ttu-id="d2d33-213">Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="d2d33-213">Customizing Site-Wide Behavior for ASP.NET Web Pages</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)