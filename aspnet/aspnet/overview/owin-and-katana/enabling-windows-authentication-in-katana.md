---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Microsoft Docs
author: MikeWasson
description: "Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Viene descritto come due scenari: Utilizzo di IIS per host Katana e l'utilizzo di HttpListener di self-hosting Kat..."
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65118331"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="77eff-104">Abilitazione dell'autenticazione di Windows in Katana</span><span class="sxs-lookup"><span data-stu-id="77eff-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="77eff-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77eff-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="77eff-106">Questo articolo illustra come abilitare l'autenticazione di Windows in Katana.</span><span class="sxs-lookup"><span data-stu-id="77eff-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="77eff-107">Viene descritto come due scenari: Usa IIS per ospitare Katana e uso di HttpListener di self-hosting Katana in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="77eff-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="77eff-108">Barry Dorrans, David Matson e Chris Ross grazie per aver letto questo articolo.</span><span class="sxs-lookup"><span data-stu-id="77eff-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="77eff-109">Katana è l'implementazione Microsoft del [OWIN](http://owin.org/), Open Web Interface for .NET.</span><span class="sxs-lookup"><span data-stu-id="77eff-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="77eff-110">È possibile leggere un'introduzione a OWIN e Katana [qui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="77eff-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="77eff-111">L'architettura OWIN ha diversi livelli:</span><span class="sxs-lookup"><span data-stu-id="77eff-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="77eff-112">Organizzatore: Gestisce il processo in cui viene eseguita la pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="77eff-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="77eff-113">Server: Apre un socket di rete e ascolta le richieste.</span><span class="sxs-lookup"><span data-stu-id="77eff-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="77eff-114">Middleware: Elabora la richiesta e risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="77eff-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="77eff-115">Katana offre attualmente due server, che supportano entrambi l'autenticazione integrata di Windows:</span><span class="sxs-lookup"><span data-stu-id="77eff-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="77eff-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="77eff-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="77eff-117">Usa IIS con la pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77eff-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="77eff-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="77eff-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="77eff-119">Viene utilizzato [System.NET. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="77eff-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="77eff-120">Questo server è attualmente l'opzione predefinita quando il Self-hosting Katana.</span><span class="sxs-lookup"><span data-stu-id="77eff-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="77eff-121">Katana attualmente non fornisce il middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nel server.</span><span class="sxs-lookup"><span data-stu-id="77eff-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="77eff-122">Autenticazione di Windows in IIS</span><span class="sxs-lookup"><span data-stu-id="77eff-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="77eff-123">Usa systemweb, è possibile semplicemente abilitare l'autenticazione di Windows in IIS.</span><span class="sxs-lookup"><span data-stu-id="77eff-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="77eff-124">Iniziamo creando una nuova applicazione ASP.NET, usando il modello di progetto "Applicazione Web ASP.NET vuota".</span><span class="sxs-lookup"><span data-stu-id="77eff-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="77eff-125">Successivamente, aggiungere i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="77eff-125">Next, add NuGet packages.</span></span> <span data-ttu-id="77eff-126">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="77eff-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="77eff-127">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="77eff-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="77eff-128">Ora aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="77eff-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="77eff-129">Questo è tutto che è necessario creare un'applicazione "Hello world" per OWIN, in esecuzione in IIS.</span><span class="sxs-lookup"><span data-stu-id="77eff-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="77eff-130">‎Premere F5 per eseguire il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="77eff-130">Press F5 to debug the application.</span></span> <span data-ttu-id="77eff-131">Si dovrebbe vedere "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="77eff-131">You should see "Hello World!"</span></span> <span data-ttu-id="77eff-132">Nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="77eff-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="77eff-133">Successivamente, si sarà abilitare l'autenticazione di Windows in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="77eff-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="77eff-134">Dal **View** dal menu **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="77eff-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="77eff-135">Fare clic sul nome del progetto in Esplora soluzioni per visualizzare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="77eff-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="77eff-136">Nel **delle proprietà** impostare nella finestra **autenticazione anonima** a **disabilitato** e impostare **l'autenticazione di Windows** a  **Abilitato**.</span><span class="sxs-lookup"><span data-stu-id="77eff-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="77eff-137">Quando si esegue l'applicazione da Visual Studio, IIS Express richiederà le credenziali di Windows dell'utente.</span><span class="sxs-lookup"><span data-stu-id="77eff-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="77eff-138">È possibile verificarlo utilizzando [Fiddler](http://fiddler2.com/home) o HTTP di un altro strumento di debug.</span><span class="sxs-lookup"><span data-stu-id="77eff-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="77eff-139">Di seguito è riportato un esempio di risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="77eff-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="77eff-140">Le intestazioni WWW-Authenticate nella risposta indicano che il server supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocollo, che usa Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="77eff-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="77eff-141">In un secondo momento, quando si distribuisce l'applicazione in un server, seguire [questi passaggi](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) per abilitare l'autenticazione di Windows in IIS su tale server.</span><span class="sxs-lookup"><span data-stu-id="77eff-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="77eff-142">Autenticazione di Windows in HttpListener</span><span class="sxs-lookup"><span data-stu-id="77eff-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="77eff-143">Se si usa Microsoft.Owin.Host.HttpListener indipendente su Katana, è possibile abilitare l'autenticazione di Windows direttamente nel **HttpListener** istanza.</span><span class="sxs-lookup"><span data-stu-id="77eff-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="77eff-144">In primo luogo, creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="77eff-144">First, create a new console application.</span></span> <span data-ttu-id="77eff-145">Successivamente, aggiungere i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="77eff-145">Next, add NuGet packages.</span></span> <span data-ttu-id="77eff-146">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="77eff-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="77eff-147">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="77eff-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="77eff-148">Ora aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="77eff-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="77eff-149">Questa classe implementa lo stesso esempio "Hello world" dalla prima, ma imposta anche l'autenticazione di Windows come schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="77eff-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="77eff-150">All'interno di `Main` di funzione, avviare la pipeline OWIN:</span><span class="sxs-lookup"><span data-stu-id="77eff-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="77eff-151">È possibile inviare una richiesta in Fiddler per confermare che l'applicazione usa l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="77eff-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="77eff-152">Argomenti correlati</span><span class="sxs-lookup"><span data-stu-id="77eff-152">Related Topics</span></span>

[<span data-ttu-id="77eff-153">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="77eff-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="77eff-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="77eff-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="77eff-155">Informazioni sull'autenticazione form OWIN in MVC 5</span><span class="sxs-lookup"><span data-stu-id="77eff-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
