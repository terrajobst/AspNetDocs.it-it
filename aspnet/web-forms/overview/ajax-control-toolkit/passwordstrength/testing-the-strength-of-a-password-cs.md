---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Verifica della complessità di una password (C#) | Microsoft Docs
author: wenz
description: Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere. Controllo PasswordStrength in ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: e55eab9feebc18f39dd40c59cfb423208296b6c5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627295"
---
# <a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="37363-104">Test della complessità di una password (C#)</span><span class="sxs-lookup"><span data-stu-id="37363-104">Testing the Strength of a Password (C#)</span></span>

<span data-ttu-id="37363-105">di [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="37363-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="37363-106">[Scarica codice](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="37363-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="37363-107">Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="37363-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="37363-108">Il controllo PasswordStrength in ASP.NET AJAX Control Toolkit può verificare l'efficacia di una password.</span><span class="sxs-lookup"><span data-stu-id="37363-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="37363-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="37363-109">Overview</span></span>

<span data-ttu-id="37363-110">Le password sono obbligatorie praticamente ovunque, in modo che gli utenti Lazy tendano a scegliere semplici password facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="37363-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="37363-111">Il controllo `PasswordStrength` in ASP.NET AJAX Control Toolkit può verificare l'efficacia di una password.</span><span class="sxs-lookup"><span data-stu-id="37363-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="37363-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="37363-112">Steps</span></span>

<span data-ttu-id="37363-113">Il controllo `PasswordStrength` estende una casella di testo e controlla se la password è sufficientemente adeguata.</span><span class="sxs-lookup"><span data-stu-id="37363-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="37363-114">Offre un'ampia gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:</span><span class="sxs-lookup"><span data-stu-id="37363-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="37363-115">`MinimumNumericCharacters` numero minimo di caratteri numerici richiesti nella password</span><span class="sxs-lookup"><span data-stu-id="37363-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="37363-116">`MinimumSymbolCharacters` numero minimo di caratteri simbolici (non lettere e cifre) richiesti nella password</span><span class="sxs-lookup"><span data-stu-id="37363-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="37363-117">`PreferredPasswordLength` lunghezza minima della password</span><span class="sxs-lookup"><span data-stu-id="37363-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="37363-118">`RequiresUpperAndLowerCaseCharacters` se la password deve usare caratteri sia maiuscoli che minuscoli</span><span class="sxs-lookup"><span data-stu-id="37363-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="37363-119">Il `StrengthIndicatorType` fornisce le informazioni su come presentare il livello di attendibilità della password, come testo (valore `"Text"`) o come indicatore di stato (valore `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="37363-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="37363-120">Nell'attributo `DisplayPosition` è possibile configurare la posizione in cui vengono visualizzate le informazioni.</span><span class="sxs-lookup"><span data-stu-id="37363-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="37363-121">Di seguito è riportato un esempio completo, tra cui il controllo `ScriptManager` AJAX ASP.NET, il controllo `PasswordStrength` e ovviamente una casella di testo in cui l'utente può immettere una password.</span><span class="sxs-lookup"><span data-stu-id="37363-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="37363-122">Ai fini della dimostrazione, il campo del secondo form è un campo di testo normale e non un campo password, in modo che sia possibile visualizzare durante lo sviluppo ciò che si sta digitando.</span><span class="sxs-lookup"><span data-stu-id="37363-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="37363-123">Esegui la pagina e digita il testo: solo dopo aver immesso lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata inviolabile.</span><span class="sxs-lookup"><span data-stu-id="37363-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="37363-124">[![ora la password è (piuttosto) corretta](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="37363-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="37363-125">A questo punto la password è (piuttosto) corretta ([fare clic per visualizzare l'immagine con dimensioni complete](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="37363-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="37363-126">avanti</span><span class="sxs-lookup"><span data-stu-id="37363-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
