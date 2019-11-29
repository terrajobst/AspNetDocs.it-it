---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: Self-host SignalR | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript. Versioni del software usate nell'esercitazione V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578570"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="39e35-104">Esercitazione: Self-host SignalR</span><span class="sxs-lookup"><span data-stu-id="39e35-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="39e35-105">di [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="39e35-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="39e35-106">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="39e35-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="39e35-107">Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39e35-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="39e35-108">Versioni del software usate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="39e35-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="39e35-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="39e35-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="39e35-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="39e35-110">.NET 4.5</span></span>
> - <span data-ttu-id="39e35-111">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="39e35-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="39e35-112">Uso di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="39e35-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="39e35-113">Per usare Visual Studio 2012 con questa esercitazione, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="39e35-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="39e35-114">Aggiornare [Gestione pacchetti](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="39e35-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="39e35-115">Installare l' [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="39e35-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="39e35-116">Nell'installazione guidata piattaforma Web cercare e installare **ASP.NET and Web Tools 2013,1 per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="39e35-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="39e35-117">In questo modo vengono installati i modelli di Visual Studio per le classi SignalR, ad esempio **Hub**.</span><span class="sxs-lookup"><span data-stu-id="39e35-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="39e35-118">Alcuni modelli, ad esempio **OWIN Startup Class**, non saranno disponibili. per questi, usare invece un file di classe.</span><span class="sxs-lookup"><span data-stu-id="39e35-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="39e35-119">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="39e35-119">Questions and comments</span></span>
>
> <span data-ttu-id="39e35-120">Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="39e35-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="39e35-121">In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="39e35-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="39e35-122">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="39e35-122">Overview</span></span>

<span data-ttu-id="39e35-123">Un server SignalR è in genere ospitato in un'applicazione ASP.NET in IIS, ma può anche essere indipendente (ad esempio in un'applicazione console o in un servizio Windows) usando la libreria Self-host.</span><span class="sxs-lookup"><span data-stu-id="39e35-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="39e35-124">Questa libreria, come all of SignalR 2, si basa su OWIN ([Open Web Interface for .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="39e35-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="39e35-125">OWIN definisce un'astrazione tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="39e35-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="39e35-126">OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS.</span><span class="sxs-lookup"><span data-stu-id="39e35-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="39e35-127">I motivi per non ospitare in IIS includono:</span><span class="sxs-lookup"><span data-stu-id="39e35-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="39e35-128">Ambienti in cui IIS non è disponibile o auspicabile, ad esempio un server farm esistente senza IIS.</span><span class="sxs-lookup"><span data-stu-id="39e35-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="39e35-129">È necessario evitare il sovraccarico delle prestazioni di IIS.</span><span class="sxs-lookup"><span data-stu-id="39e35-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="39e35-130">La funzionalità SignalR deve essere aggiunta a un'applicazione esistente eseguita in un servizio Windows, in un ruolo di lavoro di Azure o in un altro processo.</span><span class="sxs-lookup"><span data-stu-id="39e35-130">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="39e35-131">Se una soluzione viene sviluppata come Self-host per motivi di prestazioni, è consigliabile testare anche l'applicazione ospitata in IIS per determinare il vantaggio in termini di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="39e35-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="39e35-132">Questa esercitazione contiene le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="39e35-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="39e35-133">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="39e35-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="39e35-134">Accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="39e35-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="39e35-135">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="39e35-135">Creating the server</span></span>

<span data-ttu-id="39e35-136">In questa esercitazione verrà creato un server ospitato in un'applicazione console, ma il server può essere ospitato in qualsiasi tipo di processo, ad esempio un servizio Windows o un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="39e35-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="39e35-137">Per il codice di esempio per l'hosting di un server SignalR in un servizio Windows, vedere la pagina relativa all' [hosting automatico di SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="39e35-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="39e35-138">Aprire Visual Studio 2013 con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="39e35-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="39e35-139">Selezionare **file**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="39e35-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="39e35-140">Selezionare **Windows** sotto il **nodo C# visivo** nel riquadro **modelli** e selezionare il modello **applicazione console** .</span><span class="sxs-lookup"><span data-stu-id="39e35-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="39e35-141">Assegnare al nuovo progetto il nome "SignalRSelfHost" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="39e35-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="39e35-142">Aprire la console di gestione pacchetti NuGet selezionando **strumenti** > **gestione pacchetti NuGet** > **console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="39e35-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="39e35-143">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="39e35-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="39e35-144">Questo comando aggiunge al progetto le librerie a host automatico SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="39e35-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="39e35-145">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="39e35-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="39e35-146">Questo comando aggiunge la libreria Microsoft. Owin. CORS al progetto.</span><span class="sxs-lookup"><span data-stu-id="39e35-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="39e35-147">Questa libreria verrà usata per il supporto tra domini, operazione necessaria per le applicazioni che ospitano SignalR e un client di pagine Web in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="39e35-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="39e35-148">Poiché si ospiterà il server SignalR e il client Web su porte diverse, ciò significa che tra domini deve essere abilitato per la comunicazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="39e35-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="39e35-149">Sostituire il contenuto di Program.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="39e35-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="39e35-150">Il codice precedente include tre classi:</span><span class="sxs-lookup"><span data-stu-id="39e35-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="39e35-151">**Programma**, incluso il metodo **principale** che definisce il percorso principale di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="39e35-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="39e35-152">In questo metodo, un'applicazione Web di tipo **Startup** viene avviata all'URL specificato (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="39e35-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="39e35-153">Se per l'endpoint è richiesta la sicurezza, è possibile implementare SSL.</span><span class="sxs-lookup"><span data-stu-id="39e35-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="39e35-154">Per altre informazioni [, vedere Procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) .</span><span class="sxs-lookup"><span data-stu-id="39e35-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="39e35-155">**Avvio**, la classe che contiene la configurazione per il server SignalR (l'unica configurazione utilizzata in questa esercitazione è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare route per tutti gli oggetti Hub nel progetto.</span><span class="sxs-lookup"><span data-stu-id="39e35-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="39e35-156">**MyHub**, la classe dell'hub SignalR che l'applicazione fornirà ai client.</span><span class="sxs-lookup"><span data-stu-id="39e35-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="39e35-157">Questa classe dispone di un solo metodo, **Send**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="39e35-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="39e35-158">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="39e35-158">Compile and run the application.</span></span> <span data-ttu-id="39e35-159">L'indirizzo che il server sta eseguendo dovrebbe essere visualizzato in una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="39e35-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="39e35-160">Se l'esecuzione ha esito negativo con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, sarà necessario riavviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="39e35-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="39e35-161">Arrestare l'applicazione prima di procedere alla sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="39e35-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="39e35-162">Accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="39e35-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="39e35-163">In questa sezione si userà lo stesso client JavaScript dell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="39e35-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="39e35-164">Verrà apportata una sola modifica al client, ovvero definire in modo esplicito l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="39e35-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="39e35-165">Con un'applicazione indipendente, il server potrebbe non essere necessariamente nello stesso indirizzo dell'URL di connessione (a causa dei proxy inversi e dei bilanciamenti del carico), quindi l'URL deve essere definito in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="39e35-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="39e35-166">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="39e35-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="39e35-167">Selezionare il nodo **Web** e selezionare il modello **applicazione Web ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="39e35-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="39e35-168">Denominare il progetto "JavascriptClient" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="39e35-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="39e35-169">Selezionare il modello **vuoto** e lasciare deselezionate le opzioni rimanenti.</span><span class="sxs-lookup"><span data-stu-id="39e35-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="39e35-170">Selezionare **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="39e35-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="39e35-171">Nella console di gestione pacchetti selezionare il progetto "JavascriptClient" nell'elenco a discesa **progetto predefinito** ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="39e35-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="39e35-172">Questo comando consente di installare le librerie SignalR e JQuery necessarie nel client.</span><span class="sxs-lookup"><span data-stu-id="39e35-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="39e35-173">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="39e35-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="39e35-174">Selezionare il nodo **Web** e selezionare pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="39e35-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="39e35-175">Assegnare alla pagina il nome **default. html**.</span><span class="sxs-lookup"><span data-stu-id="39e35-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="39e35-176">Sostituire il contenuto della nuova pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="39e35-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="39e35-177">Verificare che i riferimenti agli script corrispondano agli script nella cartella Scripts del progetto.</span><span class="sxs-lookup"><span data-stu-id="39e35-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="39e35-178">Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta effettuata al client usato nell'esercitazione di acquisizione a stella (oltre all'aggiornamento del codice a SignalR versione 2 beta).</span><span class="sxs-lookup"><span data-stu-id="39e35-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="39e35-179">Questa riga di codice imposta in modo esplicito l'URL della connessione di base per SignalR nel server.</span><span class="sxs-lookup"><span data-stu-id="39e35-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="39e35-180">Fare clic con il pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il pulsante di opzione **progetti di avvio multipli** e impostare l' **azione** di entrambi i progetti su **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="39e35-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="39e35-181">Fare clic con il pulsante destro del mouse su "default. html" e selezionare **Imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="39e35-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="39e35-182">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="39e35-182">Run the application.</span></span> <span data-ttu-id="39e35-183">Viene avviato il server e la pagina.</span><span class="sxs-lookup"><span data-stu-id="39e35-183">The server and page will launch.</span></span> <span data-ttu-id="39e35-184">Potrebbe essere necessario ricaricare la pagina Web (oppure selezionare **continua** nel debugger) se la pagina viene caricata prima dell'avvio del server.</span><span class="sxs-lookup"><span data-stu-id="39e35-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="39e35-185">Nel browser fornire un nome utente quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="39e35-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="39e35-186">Copiare l'URL della pagina in un'altra scheda o finestra del browser e specificare un nome utente diverso.</span><span class="sxs-lookup"><span data-stu-id="39e35-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="39e35-187">Sarà possibile inviare messaggi da un riquadro del browser all'altro, come nell'esercitazione Introduzione.</span><span class="sxs-lookup"><span data-stu-id="39e35-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
