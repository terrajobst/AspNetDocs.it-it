---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Uso di un CAPTCHA per impedire ai bot di usare il sito di ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: Questo articolo illustra come usare reCAPTCHA (una misura di sicurezza) per impedire ai programmi automatizzati di eseguire attività in un Pagine Web ASP.NET (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547047"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="68e19-103">Uso di un CAPTCHA per impedire ai bot di usare il sito di ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="68e19-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="68e19-104">[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="68e19-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="68e19-105">Questo articolo illustra come usare reCAPTCHA (una misura di sicurezza) per impedire ai programmi automatici (bot) di eseguire attività in un sito Web Pagine Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="68e19-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="68e19-106">**Cosa si apprenderà:**</span><span class="sxs-lookup"><span data-stu-id="68e19-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="68e19-107">Come aggiungere un test CAPTCHA al sito.</span><span class="sxs-lookup"><span data-stu-id="68e19-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="68e19-108">Queste sono le funzionalità di ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="68e19-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="68e19-109">Helper `ReCaptcha`.</span><span class="sxs-lookup"><span data-stu-id="68e19-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="68e19-110">Le informazioni contenute in questo articolo si applicano a Pagine Web ASP.NET 1,0 e Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="68e19-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="68e19-111">Informazioni sui CAPTCHA</span><span class="sxs-lookup"><span data-stu-id="68e19-111">About CAPTCHAs</span></span>

<span data-ttu-id="68e19-112">Ogni volta che si consente agli utenti di effettuare la registrazione nel sito o anche di immettere un nome e un URL (ad esempio per un commento di Blog), è possibile che si ottenga un diluvio di nomi fittizi.</span><span class="sxs-lookup"><span data-stu-id="68e19-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="68e19-113">Questi vengono spesso lasciati da programmi automatici (bot) che provano a lasciare gli URL in ogni sito Web che è in grado di trovare.</span><span class="sxs-lookup"><span data-stu-id="68e19-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="68e19-114">Un motivo comune è quello di pubblicare gli URL dei prodotti per la vendita.</span><span class="sxs-lookup"><span data-stu-id="68e19-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="68e19-115">È possibile assicurarsi che un utente sia una persona reale e non un programma per computer utilizzando un *captcha* per convalidare gli utenti durante la registrazione o in altro modo immettere il nome e il sito.</span><span class="sxs-lookup"><span data-stu-id="68e19-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="68e19-116">CAPTCHA si basa sul test di Turing pubblico completamente automatizzato per comunicare a computer e uomini.</span><span class="sxs-lookup"><span data-stu-id="68e19-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="68e19-117">Un CAPTCHA è un test di *richiesta-risposta* in cui all'utente viene richiesto di eseguire un'operazione semplice, ma è difficile per un programma automatizzato.</span><span class="sxs-lookup"><span data-stu-id="68e19-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="68e19-118">Il tipo più comune di CAPTCHA è quello in cui vengono visualizzate alcune lettere distorte e viene richiesto di digitarle.</span><span class="sxs-lookup"><span data-stu-id="68e19-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="68e19-119">(La distorsione dovrebbe rendere difficile per i bot decifrare le lettere).</span><span class="sxs-lookup"><span data-stu-id="68e19-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="68e19-120">Aggiunta di un test reCAPTCHA</span><span class="sxs-lookup"><span data-stu-id="68e19-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="68e19-121">Nelle pagine di ASP.NET è possibile usare l'helper `ReCaptcha` per eseguire il rendering di un test CAPTCHA basato sul servizio reCAPTCHA ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="68e19-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="68e19-122">L'helper `ReCaptcha` Visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina venga convalidata.</span><span class="sxs-lookup"><span data-stu-id="68e19-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="68e19-123">La risposta dell'utente viene convalidata dal servizio ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="68e19-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="68e19-124">Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="68e19-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="68e19-125">Al termine della registrazione, si otterranno una chiave pubblica e una chiave privata.</span><span class="sxs-lookup"><span data-stu-id="68e19-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="68e19-126">Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="68e19-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="68e19-127">Se non si dispone già di un file *\_AppStart. cshtml* , nella cartella radice di un sito Web creare un file denominato *\_AppStart. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="68e19-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="68e19-128">Aggiungere le seguenti impostazioni Helper `Recaptcha` nel file *\_AppStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="68e19-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="68e19-129">Impostare le proprietà `PublicKey` e `PrivateKey` usando le chiavi pubbliche e private.</span><span class="sxs-lookup"><span data-stu-id="68e19-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="68e19-130">Salvare il file *\_AppStart. cshtml* e chiuderlo.</span><span class="sxs-lookup"><span data-stu-id="68e19-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="68e19-131">Nella cartella radice di un sito Web, creare una nuova pagina denominata *recaptcha. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="68e19-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="68e19-132">Sostituire il contenuto esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="68e19-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="68e19-133">Eseguire la pagina *recaptcha. cshtml* in un browser.</span><span class="sxs-lookup"><span data-stu-id="68e19-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="68e19-134">Se il valore `PrivateKey` è valido, nella pagina vengono visualizzati il controllo reCAPTCHA e un pulsante.</span><span class="sxs-lookup"><span data-stu-id="68e19-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="68e19-135">Se le chiavi non sono state impostate a livello globale in *\_AppStart. html*, nella pagina verrà visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="68e19-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="68e19-136">Immettere le parole per il test.</span><span class="sxs-lookup"><span data-stu-id="68e19-136">Enter the words for the test.</span></span> <span data-ttu-id="68e19-137">Se si passa il test reCAPTCHA, viene visualizzato un messaggio di questo effetto.</span><span class="sxs-lookup"><span data-stu-id="68e19-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="68e19-138">In caso contrario, viene visualizzato un messaggio di errore e il controllo reCAPTCHA viene nuovamente visualizzato.</span><span class="sxs-lookup"><span data-stu-id="68e19-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="68e19-139">Se il computer si trova in un dominio che utilizza il server proxy, potrebbe essere necessario configurare l'elemento `defaultproxy` del file *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="68e19-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="68e19-140">Nell'esempio seguente viene illustrato un file *Web. config* con l'elemento `defaultproxy` configurato per consentire il funzionamento del servizio reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="68e19-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="68e19-141">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="68e19-141">Additional Resources</span></span>

- [<span data-ttu-id="68e19-142">Personalizzazione del comportamento a livello di sito per siti Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68e19-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="68e19-143">Ricaptcha sito</span><span class="sxs-lookup"><span data-stu-id="68e19-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
