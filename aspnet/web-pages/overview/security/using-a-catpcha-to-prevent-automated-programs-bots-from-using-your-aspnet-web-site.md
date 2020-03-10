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
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Uso di un CAPTCHA per impedire ai bot di usare il sito di ASP.NET Web Razor)

[Microsoft](https://github.com/microsoft)

> Questo articolo illustra come usare reCAPTCHA (una misura di sicurezza) per impedire ai programmi automatici (bot) di eseguire attività in un sito Web Pagine Web ASP.NET (Razor).
> 
> **Cosa si apprenderà:** 
> 
> - Come aggiungere un test CAPTCHA al sito.
> 
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
> 
> - Helper `ReCaptcha`.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a Pagine Web ASP.NET 1,0 e Web Pages 2.

## <a name="about-captchas"></a>Informazioni sui CAPTCHA

Ogni volta che si consente agli utenti di effettuare la registrazione nel sito o anche di immettere un nome e un URL (ad esempio per un commento di Blog), è possibile che si ottenga un diluvio di nomi fittizi. Questi vengono spesso lasciati da programmi automatici (bot) che provano a lasciare gli URL in ogni sito Web che è in grado di trovare. Un motivo comune è quello di pubblicare gli URL dei prodotti per la vendita.

È possibile assicurarsi che un utente sia una persona reale e non un programma per computer utilizzando un *captcha* per convalidare gli utenti durante la registrazione o in altro modo immettere il nome e il sito. CAPTCHA si basa sul test di Turing pubblico completamente automatizzato per comunicare a computer e uomini. Un CAPTCHA è un test di *richiesta-risposta* in cui all'utente viene richiesto di eseguire un'operazione semplice, ma è difficile per un programma automatizzato. Il tipo più comune di CAPTCHA è quello in cui vengono visualizzate alcune lettere distorte e viene richiesto di digitarle. (La distorsione dovrebbe rendere difficile per i bot decifrare le lettere).

## <a name="adding-a-recaptcha-test"></a>Aggiunta di un test reCAPTCHA

Nelle pagine di ASP.NET è possibile usare l'helper `ReCaptcha` per eseguire il rendering di un test CAPTCHA basato sul servizio reCAPTCHA ([http://recaptcha.net](http://recaptcha.net)). L'helper `ReCaptcha` Visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina venga convalidata. La risposta dell'utente viene convalidata dal servizio ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Al termine della registrazione, si otterranno una chiave pubblica e una chiave privata.
2. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
3. Se non si dispone già di un file *\_AppStart. cshtml* , nella cartella radice di un sito Web creare un file denominato *\_AppStart. cshtml*.
4. Aggiungere le seguenti impostazioni Helper `Recaptcha` nel file *\_AppStart. cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Impostare le proprietà `PublicKey` e `PrivateKey` usando le chiavi pubbliche e private.
6. Salvare il file *\_AppStart. cshtml* e chiuderlo.
7. Nella cartella radice di un sito Web, creare una nuova pagina denominata *recaptcha. cshtml*.
8. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Eseguire la pagina *recaptcha. cshtml* in un browser. Se il valore `PrivateKey` è valido, nella pagina vengono visualizzati il controllo reCAPTCHA e un pulsante. Se le chiavi non sono state impostate a livello globale in *\_AppStart. html*, nella pagina verrà visualizzato un errore. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Immettere le parole per il test. Se si passa il test reCAPTCHA, viene visualizzato un messaggio di questo effetto. In caso contrario, viene visualizzato un messaggio di errore e il controllo reCAPTCHA viene nuovamente visualizzato.

> [!NOTE]
> Se il computer si trova in un dominio che utilizza il server proxy, potrebbe essere necessario configurare l'elemento `defaultproxy` del file *Web. config* . Nell'esempio seguente viene illustrato un file *Web. config* con l'elemento `defaultproxy` configurato per consentire il funzionamento del servizio reCAPTCHA.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Personalizzazione del comportamento a livello di sito per siti Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Ricaptcha sito](https://www.google.com/recaptcha)
