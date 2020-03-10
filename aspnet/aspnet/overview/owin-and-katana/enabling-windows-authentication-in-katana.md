---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Microsoft Docs
author: MikeWasson
description: "Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Vengono illustrati due scenari: l'utilizzo di IIS per ospitare la Katana e l'utilizzo di HttpListener per l'hosting automatico di Kat..."
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617180"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="9c0db-104">Abilitazione dell'autenticazione di Windows in Katana</span><span class="sxs-lookup"><span data-stu-id="9c0db-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="9c0db-105">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9c0db-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="9c0db-106">Questo articolo illustra come abilitare l'autenticazione di Windows in Katana.</span><span class="sxs-lookup"><span data-stu-id="9c0db-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="9c0db-107">Vengono illustrati due scenari: l'uso di IIS per ospitare la Katana e l'uso di HttpListener per ospitare in modo automatico la katana in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="9c0db-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="9c0db-108">Grazie a Barry Dorrans, David Matson e Chris Ross per la revisione di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="9c0db-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="9c0db-109">Katana è l'implementazione Microsoft di [OWIN](http://owin.org/), l'interfaccia Web aperta per .NET.</span><span class="sxs-lookup"><span data-stu-id="9c0db-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="9c0db-110">È possibile leggere un'introduzione a OWIN e Katana [qui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="9c0db-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="9c0db-111">L'architettura OWIN presenta diversi livelli:</span><span class="sxs-lookup"><span data-stu-id="9c0db-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="9c0db-112">Host: gestisce il processo in cui viene eseguita la pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="9c0db-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="9c0db-113">Server: apre un socket di rete e resta in attesa di richieste.</span><span class="sxs-lookup"><span data-stu-id="9c0db-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="9c0db-114">Middleware: elabora la richiesta e la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c0db-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="9c0db-115">Katana fornisce attualmente due server, entrambi supportati dall'autenticazione integrata di Windows:</span><span class="sxs-lookup"><span data-stu-id="9c0db-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="9c0db-116">**Microsoft. Owin. host. systemWeb**.</span><span class="sxs-lookup"><span data-stu-id="9c0db-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="9c0db-117">Usa IIS con la pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9c0db-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="9c0db-118">**Microsoft. Owin. host. HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="9c0db-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="9c0db-119">USA [System .NET. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c0db-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="9c0db-120">Questo server è attualmente l'opzione predefinita quando si esegue l'hosting automatico di katana.</span><span class="sxs-lookup"><span data-stu-id="9c0db-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="9c0db-121">Katana non fornisce attualmente il middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nei server.</span><span class="sxs-lookup"><span data-stu-id="9c0db-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="9c0db-122">Autenticazione di Windows in IIS</span><span class="sxs-lookup"><span data-stu-id="9c0db-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="9c0db-123">Con Microsoft. Owin. host. SystemWeb è possibile abilitare semplicemente l'autenticazione di Windows in IIS.</span><span class="sxs-lookup"><span data-stu-id="9c0db-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="9c0db-124">Per iniziare, creare una nuova applicazione ASP.NET usando il modello di progetto "applicazione Web vuota ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="9c0db-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="9c0db-125">Aggiungere quindi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="9c0db-125">Next, add NuGet packages.</span></span> <span data-ttu-id="9c0db-126">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="9c0db-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9c0db-127">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9c0db-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="9c0db-128">A questo punto aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9c0db-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="9c0db-129">Questo è tutto ciò che serve per creare un'applicazione "Hello World" per OWIN, in esecuzione in IIS.</span><span class="sxs-lookup"><span data-stu-id="9c0db-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="9c0db-130">‎Premere F5 per eseguire il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c0db-130">Press F5 to debug the application.</span></span> <span data-ttu-id="9c0db-131">Dovrebbe essere visualizzato "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9c0db-131">You should see "Hello World!"</span></span> <span data-ttu-id="9c0db-132">nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="9c0db-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="9c0db-133">Successivamente, si Abilita l'autenticazione di Windows in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9c0db-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="9c0db-134">Scegliere **Proprietà**dal menu **Visualizza** .</span><span class="sxs-lookup"><span data-stu-id="9c0db-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="9c0db-135">Per visualizzare le proprietà del progetto, fare clic sul nome del progetto in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="9c0db-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="9c0db-136">Nella finestra **Proprietà** impostare **autenticazione anonima** su **disabilitato** e impostare **autenticazione di Windows** su **abilitato**.</span><span class="sxs-lookup"><span data-stu-id="9c0db-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="9c0db-137">Quando l'applicazione viene eseguita da Visual Studio, IIS Express richiederà le credenziali di Windows dell'utente.</span><span class="sxs-lookup"><span data-stu-id="9c0db-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="9c0db-138">Per visualizzarlo, è possibile usare [Fiddler](http://fiddler2.com/home) o un altro strumento di debug http.</span><span class="sxs-lookup"><span data-stu-id="9c0db-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="9c0db-139">Di seguito è riportato un esempio di risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="9c0db-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="9c0db-140">Le intestazioni WWW-Authenticate in questa risposta indicano che il server supporta il protocollo [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , che usa Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="9c0db-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="9c0db-141">Successivamente, quando si distribuisce l'applicazione in un server, attenersi alla [procedura](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) seguente per abilitare l'autenticazione di Windows in IIS su tale server.</span><span class="sxs-lookup"><span data-stu-id="9c0db-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="9c0db-142">Autenticazione di Windows in HttpListener</span><span class="sxs-lookup"><span data-stu-id="9c0db-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="9c0db-143">Se si usa Microsoft. Owin. host. HttpListener per ospitare in modo autonomo la katana, è possibile abilitare l'autenticazione di Windows direttamente nell'istanza di **HttpListener** .</span><span class="sxs-lookup"><span data-stu-id="9c0db-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="9c0db-144">Per prima cosa, creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="9c0db-144">First, create a new console application.</span></span> <span data-ttu-id="9c0db-145">Aggiungere quindi i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="9c0db-145">Next, add NuGet packages.</span></span> <span data-ttu-id="9c0db-146">Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="9c0db-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9c0db-147">Nella finestra Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9c0db-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="9c0db-148">A questo punto aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9c0db-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="9c0db-149">Questa classe implementa lo stesso esempio "Hello World" di prima, ma imposta anche l'autenticazione di Windows come schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9c0db-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="9c0db-150">All'interno della funzione `Main`, avviare la pipeline OWIN:</span><span class="sxs-lookup"><span data-stu-id="9c0db-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="9c0db-151">È possibile inviare una richiesta in Fiddler per confermare che l'applicazione usa l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="9c0db-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="9c0db-152">Argomenti correlati</span><span class="sxs-lookup"><span data-stu-id="9c0db-152">Related Topics</span></span>

[<span data-ttu-id="9c0db-153">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="9c0db-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="9c0db-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="9c0db-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="9c0db-155">Informazioni sull'autenticazione basata su form OWIN in MVC 5</span><span class="sxs-lookup"><span data-stu-id="9c0db-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
