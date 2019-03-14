---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Scelta dell'approccio corretto per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Quando si lavora con Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, sono presenti tre approcci principali, che è possibile usare per ottenere...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0b21852a1db2862a8452e332021b55ce7f1db423
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036318"
---
<a name="choosing-the-right-approach-to-web-deployment"></a><span data-ttu-id="2d425-103">Scelta dell'approccio corretto per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="2d425-103">Choosing the Right Approach to Web Deployment</span></span>
====================
<span data-ttu-id="2d425-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2d425-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2d425-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="2d425-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2d425-106">Quando si lavora con Internet Information Services (IIS) strumento di distribuzione Web (distribuzione Web) 2.0 o versione successiva, esistono tre approcci principali, che è possibile usare per ottenere le applicazioni web nel pacchetto in un server web.</span><span class="sxs-lookup"><span data-stu-id="2d425-106">When you work with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your packaged web applications onto a web server.</span></span> <span data-ttu-id="2d425-107">È possibile:</span><span class="sxs-lookup"><span data-stu-id="2d425-107">You can either:</span></span>
> 
> - <span data-ttu-id="2d425-108">Distribuire l'applicazione da una posizione remota usando come destinazione il *servizio agente distribuzione Web* (noto anche come "remoto agente") nel server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-108">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the "remote agent") on the destination server.</span></span>
> - <span data-ttu-id="2d425-109">Distribuire l'applicazione da una posizione remota usando distribuzione Web su richiesta (noto anche come "temp agente").</span><span class="sxs-lookup"><span data-stu-id="2d425-109">Deploy the application from a remote location using Web Deploy On Demand (also known as the "temp agent").</span></span>
> - <span data-ttu-id="2d425-110">Distribuire l'applicazione da una posizione remota usando come destinazione il *gestore distribuzione Web di IIS* nel server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-110">Deploy the application from a remote location by targeting the *IIS Web Deploy Handler* on the destination server.</span></span>
> - <span data-ttu-id="2d425-111">Distribuire l'applicazione quando si copia il pacchetto web nel server di destinazione e importarlo tramite Gestione IIS manualmente.</span><span class="sxs-lookup"><span data-stu-id="2d425-111">Deploy the application by manually copying the web package to the destination server and importing it through IIS Manager.</span></span>
> 
> <span data-ttu-id="2d425-112">Come configurare i server web di destinazione varia in base quale approccio alla distribuzione da usare.</span><span class="sxs-lookup"><span data-stu-id="2d425-112">How you configure your destination web servers will depend on which approach to deployment you want to use.</span></span> <span data-ttu-id="2d425-113">Questo argomento consente di decidere quale approccio alla distribuzione è adatta a te.</span><span class="sxs-lookup"><span data-stu-id="2d425-113">This topic will help you decide which approach to deployment is right for you.</span></span>


<span data-ttu-id="2d425-114">Questa tabella illustra i principali vantaggi e svantaggi di ogni approccio di distribuzione, con gli scenari più comuni in base ciascun approccio.</span><span class="sxs-lookup"><span data-stu-id="2d425-114">This table shows the main advantages and disadvantages of each deployment approach, together with the scenarios that most typically suit each approach.</span></span>

| <span data-ttu-id="2d425-115">Approccio</span><span class="sxs-lookup"><span data-stu-id="2d425-115">Approach</span></span> | <span data-ttu-id="2d425-116">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="2d425-116">Advantages</span></span> | <span data-ttu-id="2d425-117">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="2d425-117">Disadvantages</span></span> | <span data-ttu-id="2d425-118">Scenari tipici</span><span class="sxs-lookup"><span data-stu-id="2d425-118">Typical Scenarios</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2d425-119">Agente remoto</span><span class="sxs-lookup"><span data-stu-id="2d425-119">Remote Agent</span></span> | <span data-ttu-id="2d425-120">È semplice da configurare.</span><span class="sxs-lookup"><span data-stu-id="2d425-120">It is easy to set up.</span></span> <span data-ttu-id="2d425-121">È adatto per gli aggiornamenti periodici per le applicazioni web e il contenuto.</span><span class="sxs-lookup"><span data-stu-id="2d425-121">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="2d425-122">L'utente deve essere un amministratore nel server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-122">The user must be an administrator on the target server.</span></span> <span data-ttu-id="2d425-123">l'utente non è possibile fornire credenziali alternative.</span><span class="sxs-lookup"><span data-stu-id="2d425-123">the user can't supply alternative credentials.</span></span> | <span data-ttu-id="2d425-124">Ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="2d425-124">Development environments.</span></span> <span data-ttu-id="2d425-125">Ambienti di test.</span><span class="sxs-lookup"><span data-stu-id="2d425-125">Test environments.</span></span> |
| <span data-ttu-id="2d425-126">Agente TEMP</span><span class="sxs-lookup"><span data-stu-id="2d425-126">Temp Agent</span></span> | <span data-ttu-id="2d425-127">Non è necessario per installare distribuzione Web nel computer di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-127">There is no need to install Web Deploy on the target computer.</span></span> <span data-ttu-id="2d425-128">La versione più recente di distribuzione Web viene utilizzata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2d425-128">The latest version of Web Deploy is automatically used.</span></span> | <span data-ttu-id="2d425-129">L'utente deve essere un amministratore nel server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-129">The user must be an administrator on the target server.</span></span> <span data-ttu-id="2d425-130">l'utente non è possibile fornire credenziali alternative.</span><span class="sxs-lookup"><span data-stu-id="2d425-130">The user can't supply alternative credentials.</span></span> | <span data-ttu-id="2d425-131">Ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="2d425-131">Development environments.</span></span> <span data-ttu-id="2d425-132">Ambienti di test.</span><span class="sxs-lookup"><span data-stu-id="2d425-132">Test environments.</span></span> |
| <span data-ttu-id="2d425-133">Gestore di distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="2d425-133">Web Deploy Handler</span></span> | <span data-ttu-id="2d425-134">Gli utenti non amministratori possono distribuire il contenuto.</span><span class="sxs-lookup"><span data-stu-id="2d425-134">Non-administrator users can deploy content.</span></span> <span data-ttu-id="2d425-135">È adatto per gli aggiornamenti periodici per le applicazioni web e il contenuto.</span><span class="sxs-lookup"><span data-stu-id="2d425-135">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="2d425-136">È molto più complessa da configurare.</span><span class="sxs-lookup"><span data-stu-id="2d425-136">It is a lot more complex to set up.</span></span> | <span data-ttu-id="2d425-137">Ambienti di staging.</span><span class="sxs-lookup"><span data-stu-id="2d425-137">Staging environments.</span></span> <span data-ttu-id="2d425-138">Ambienti di produzione di Intranet.</span><span class="sxs-lookup"><span data-stu-id="2d425-138">Intranet production environments.</span></span> <span data-ttu-id="2d425-139">Ambienti host.</span><span class="sxs-lookup"><span data-stu-id="2d425-139">Hosted environments.</span></span> |
| <span data-ttu-id="2d425-140">Distribuzione offline</span><span class="sxs-lookup"><span data-stu-id="2d425-140">Offline Deployment</span></span> | <span data-ttu-id="2d425-141">È molto semplice da configurare.</span><span class="sxs-lookup"><span data-stu-id="2d425-141">It is very easy to set up.</span></span> <span data-ttu-id="2d425-142">È adatto per ambienti isolati.</span><span class="sxs-lookup"><span data-stu-id="2d425-142">It is suitable for isolated environments.</span></span> | <span data-ttu-id="2d425-143">L'amministratore del server deve copiare e importare il pacchetto web ogni volta manualmente.</span><span class="sxs-lookup"><span data-stu-id="2d425-143">The server administrator must manually copy and import the web package every time.</span></span> | <span data-ttu-id="2d425-144">Ambienti di produzione con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="2d425-144">Internet-facing production environments.</span></span> <span data-ttu-id="2d425-145">Ambienti di rete isolato.</span><span class="sxs-lookup"><span data-stu-id="2d425-145">Isolated network environments.</span></span> |
  

## <a name="using-the-remote-agent"></a><span data-ttu-id="2d425-146">Usa l'agente remoto</span><span class="sxs-lookup"><span data-stu-id="2d425-146">Using the Remote Agent</span></span>

<span data-ttu-id="2d425-147">Quando si installa utilizzando le impostazioni predefinite in un server di destinazione di distribuzione Web, il servizio agente di distribuzione Web (il "agente remoto") viene automaticamente installato e avviato.</span><span class="sxs-lookup"><span data-stu-id="2d425-147">When you install Web Deploy using the default settings on a destination server, the Web Deployment Agent Service (the "remote agent") is automatically installed and started.</span></span> <span data-ttu-id="2d425-148">Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:</span><span class="sxs-lookup"><span data-stu-id="2d425-148">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="2d425-149">È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.</span><span class="sxs-lookup"><span data-stu-id="2d425-149">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="2d425-150">Gli amministratori del server è possono distribuire pacchetti web da una posizione remota, ad esempio un computer di sviluppo o un server di compilazione specificando l'indirizzo dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="2d425-150">Server administrators can deploy web packages from a remote location, like a developer machine or a build server, by specifying this endpoint address.</span></span> <span data-ttu-id="2d425-151">Si supponga, ad esempio, che Matt Hink Fabrikam, Inc. ha compilato il progetto di applicazione web ContactManager.Mvc nel suo computer dello sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="2d425-151">For example, suppose Matt Hink at Fabrikam, Inc. has built the ContactManager.Mvc web application project on his developer machine.</span></span> <span data-ttu-id="2d425-152">Il processo di compilazione genera un pacchetto web, insieme a un *. deploy. cmd* file che contiene i comandi di distribuzione Web necessari per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2d425-152">The build process generates a web package, together with a *.deploy.cmd* file that contains the Web Deploy commands required to install the package.</span></span> <span data-ttu-id="2d425-153">Se Matt è un amministratore del server nel server TESTWEB1, è possibile distribuire l'applicazione web sul server di prova web eseguendo questo comando nel suo computer dello sviluppatore:</span><span class="sxs-lookup"><span data-stu-id="2d425-153">If Matt is a server administrator on the TESTWEB1 server, he can deploy the web application to the test web server by running this command on his developer machine:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


<span data-ttu-id="2d425-154">In effetti, l'eseguibile di Web Deploy può dedurre l'indirizzo dell'endpoint dell'agente remoto, se si specifica il nome del computer, in modo Matt è sufficiente digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2d425-154">In actual fact, the Web Deploy executable can infer the endpoint address of the remote agent if you provide the machine name, so Matt only needs to type this:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="2d425-155">Per altre informazioni sulla sintassi della riga di comando di distribuzione Web e *. deploy. cmd* i file, vedere [come: Installare un pacchetto di distribuzione usando il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-155">For more information on Web Deploy command-line syntax and *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="2d425-156">L'agente remoto offre un modo semplice per distribuire il contenuto da una posizione remota, e questo approccio può funzionare bene con la distribuzione di un solo clic o automatizzata.</span><span class="sxs-lookup"><span data-stu-id="2d425-156">The remote agent offers a straightforward way to deploy content from a remote location, and this approach can work well with one-click or automated deployment.</span></span> <span data-ttu-id="2d425-157">Tuttavia, l'utente che esegue il comando di distribuzione deve essere anche amministratore di dominio o un membro del gruppo administrators locale nel server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-157">However, the user who runs the deployment command must also be either a domain administrator or a member of the local administrators group on the destination server.</span></span> <span data-ttu-id="2d425-158">Inoltre, l'agente remoto non supporta l'autenticazione di base, pertanto non è possibile passare credenziali alternative nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2d425-158">In addition, the remote agent doesn't support basic authentication, so you can't pass alternative credentials on the command line.</span></span>

<span data-ttu-id="2d425-159">L'agente remoto offre un approccio utile per la distribuzione negli scenari di test, in cui non è insolito per gli sviluppatori di controllo amministratore completo su un ambiente di server di test e le applicazioni sono in genere ricompilate e ridistribuite molto o lo sviluppo di frequente.</span><span class="sxs-lookup"><span data-stu-id="2d425-159">The remote agent provides a useful approach to deployment in development or test scenarios, where it's not uncommon for developers to have full administrator control over a test server environment, and applications are typically rebuilt and redeployed very frequently.</span></span> <span data-ttu-id="2d425-160">Tuttavia, questo approccio è in genere meno accettabile per ambienti di gestione temporanea o produzione.</span><span class="sxs-lookup"><span data-stu-id="2d425-160">However, this approach is usually less acceptable for staging or production environments.</span></span>

<span data-ttu-id="2d425-161">Per un esempio end-to-end di uno scenario che usa l'approccio dell'agente remoto, vedere [Scenario: Configurazione di un ambiente di Test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2d425-161">For an end-to-end example of a scenario that uses the remote agent approach, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span>

## <a name="using-the-temp-agent"></a><span data-ttu-id="2d425-162">Tramite l'agente Temp</span><span class="sxs-lookup"><span data-stu-id="2d425-162">Using the Temp Agent</span></span>

<span data-ttu-id="2d425-163">L'approccio agente temporanea per la distribuzione è simile all'approccio dell'agente remoto.</span><span class="sxs-lookup"><span data-stu-id="2d425-163">The temp agent approach to deployment is similar to the remote agent approach.</span></span> <span data-ttu-id="2d425-164">Tuttavia, a differenza dell'approccio di agente remoto, non devi installare distribuzione Web nel server web di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-164">However, in contrast to the remote agent approach, you don't need to install Web Deploy on the destination web server.</span></span> <span data-ttu-id="2d425-165">Al contrario, quando si esegue la distribuzione, distribuzione Web installerà una versione temporanea del servizio agente distribuzione web nel server di destinazione e verrà usato per distribuire il contenuto in IIS.</span><span class="sxs-lookup"><span data-stu-id="2d425-165">Instead, when you perform the deployment, Web Deploy will install a temporary version of the web deployment agent service on the destination server and will use this to deploy your content to IIS.</span></span> <span data-ttu-id="2d425-166">Una volta completata la distribuzione, vengono rimossi tutti i file temporanei.</span><span class="sxs-lookup"><span data-stu-id="2d425-166">When the deployment is complete, all temporary files are removed.</span></span>

<span data-ttu-id="2d425-167">Se si desidera utilizzare l'impostazione del provider agente temp, aggiungere il **/g** flag al comando di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="2d425-167">If you want to use the temp agent provider setting, add the **/g** flag to your deployment command:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="2d425-168">È possibile usare l'agente temporanea se il servizio agente di distribuzione web è installato nel computer di destinazione, anche se il servizio non è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2d425-168">You can't use the temp agent if the web deployment agent service is installed on the destination computer, even if the service is not running.</span></span>


<span data-ttu-id="2d425-169">Il vantaggio di questo approccio è che non è necessario gestire installazioni di distribuzione Web sui server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-169">The advantage of this approach is that you don't need to maintain installations of Web Deploy on your destination servers.</span></span> <span data-ttu-id="2d425-170">Inoltre, non è necessario assicurarsi che il computer di origine e destinazione siano in esecuzione la stessa versione di distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="2d425-170">Furthermore, you don't need to ensure that the source and destination computers are running the same version of Web Deploy.</span></span> <span data-ttu-id="2d425-171">Tuttavia, questo approccio presenta le stesse limitazioni dell'entità come l'approccio dell'agente remoto, vale a dire che è necessario essere un amministratore locale nel server di destinazione per poter distribuire il contenuto è supportato solo l'autenticazione NTLM.</span><span class="sxs-lookup"><span data-stu-id="2d425-171">However, this approach suffers from the same principal limitations as the remote agent approach, namely that you must be a local administrator on the destination server in order to deploy content, and only NTLM authentication is supported.</span></span> <span data-ttu-id="2d425-172">L'approccio temp agente richiede anche la configurazione iniziale di molte altre operazioni dell'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-172">The temp agent approach also requires a lot more initial configuration of the destination environment.</span></span>

<span data-ttu-id="2d425-173">Per altre informazioni sull'uso dell'agente temporanea, vedere [come: Installare un pacchetto di distribuzione usando il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) e [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-173">For more information on using the temp agent, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx) and [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

## <a name="using-the-web-deploy-handler"></a><span data-ttu-id="2d425-174">Tramite il Web distribuire gestore</span><span class="sxs-lookup"><span data-stu-id="2d425-174">Using the Web Deploy Handler</span></span>

<span data-ttu-id="2d425-175">Per IIS 7 e versioni successive, distribuzione Web offre un approccio alternativo alla distribuzione tramite il gestore di distribuzione Web di IIS.</span><span class="sxs-lookup"><span data-stu-id="2d425-175">For IIS 7 onwards, Web Deploy offers an alternative deployment approach through the IIS Web Deploy Handler.</span></span> <span data-ttu-id="2d425-176">Il gestore di distribuzione Web sono strettamente integrato con il servizio di gestione Web (WMSvc), di IIS che è progettato per consentire agli utenti di gestire siti Web IIS da posizioni remote.</span><span class="sxs-lookup"><span data-stu-id="2d425-176">The Web Deploy Handler is closely integrated with the IIS Web Management Service (WMSvc), which is designed to allow users to manage IIS websites from remote locations.</span></span>

<span data-ttu-id="2d425-177">Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:</span><span class="sxs-lookup"><span data-stu-id="2d425-177">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="2d425-178">È possibile sostituire [*server*] con il nome del computer del server web, un indirizzo IP per il server web o un nome host che si risolve nel server web.</span><span class="sxs-lookup"><span data-stu-id="2d425-178">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="2d425-179">Il grande vantaggio del gestore distribuzione Web tramite l'agente remoto e l'agente temporanea, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore distribuire le applicazioni e il contenuto a specifici siti Web IIS.</span><span class="sxs-lookup"><span data-stu-id="2d425-179">The big advantage of the Web Deploy Handler over the remote agent, and the temp agent, is that you can configure IIS to allow non-administrator users to deploy applications and content to specific IIS websites.</span></span> <span data-ttu-id="2d425-180">Il gestore di distribuzione Web supporta inoltre l'autenticazione di base, pertanto è possibile fornire credenziali alternative come parametri nei comandi di distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="2d425-180">The Web Deploy Handler also supports basic authentication, so you can provide alternative credentials as parameters in your Web Deploy commands.</span></span> <span data-ttu-id="2d425-181">Il principale svantaggio è che il gestore di distribuzione Web è inizialmente molto più complicato da configurare.</span><span class="sxs-lookup"><span data-stu-id="2d425-181">The major drawback is that the Web Deploy Handler is initially a lot more complicated to set up and configure.</span></span>

<span data-ttu-id="2d425-182">Nel caso degli utenti senza privilegi di amministratore, il servizio di gestione Web (WMSvc) consentirà solo l'utente per la connessione a IIS tramite una connessione a livello di sito, anziché una connessione a livello di server.</span><span class="sxs-lookup"><span data-stu-id="2d425-182">In the case of non-administrator users, the Web Management Service (WMSvc) will only allow the user to connect to IIS using a site-level connection, rather than a server-level connection.</span></span> <span data-ttu-id="2d425-183">Per accedere a un particolare sito, è possibile includere una stringa di query specifico del sito nell'indirizzo dell'endpoint:</span><span class="sxs-lookup"><span data-stu-id="2d425-183">To access a particular site, you can include a site-specific query string in the endpoint address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


<span data-ttu-id="2d425-184">Si supponga, ad esempio, che un processo di compilazione è configurato per distribuire automaticamente un'applicazione web in un ambiente di staging dopo ogni compilazione riuscita.</span><span class="sxs-lookup"><span data-stu-id="2d425-184">For example, suppose a build process is configured to automatically deploy a web application to a staging environment after every successful build.</span></span> <span data-ttu-id="2d425-185">Se si usa l'approccio dell'agente remoto, è necessario configurare l'identità del processo di compilazione come amministratore nei server di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2d425-185">If you used the remote agent approach, you'd need to make the build process identity an administrator on your destination servers.</span></span> <span data-ttu-id="2d425-186">Al contrario, Usa l'approccio gestore distribuzione Web è possibile assegnare un utente senza privilegi di amministratore&#x2014;**FABRIKAM\stagingdeployer** in questo caso&#x2014;possono fornire queste autorizzazioni per un solo sito specifico di IIS e il processo di compilazione credenziali per distribuire il pacchetto di web.</span><span class="sxs-lookup"><span data-stu-id="2d425-186">In contrast, using the Web Deploy Handler approach you can give a non-administrator user&#x2014;**FABRIKAM\stagingdeployer** in this case&#x2014;permission to a specific IIS website only, and the build process can provide these credentials to deploy the web package.</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> <span data-ttu-id="2d425-187">Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-187">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="2d425-188">Per altre informazioni sull'uso di *. deploy. cmd* del file, vedere [come: Installare un pacchetto di distribuzione usando il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-188">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="2d425-189">Il gestore di distribuzione Web fornisce un approccio utile per la distribuzione in ambienti, ambienti host e gli ambienti di produzione basati su intranet, in cui è disponibile l'accesso remoto al server, ma non sono credenziali di amministratore di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="2d425-189">The Web Deploy Handler provides a useful approach to deployment in staging environments, hosted environments, and intranet-based production environments, where remote access to the server is available but administrator credentials are not.</span></span>

<span data-ttu-id="2d425-190">Per un esempio end-to-end di uno scenario che usa l'approccio gestore distribuzione Web, vedere [Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2d425-190">For an end-to-end example of a scenario that uses the Web Deploy Handler approach, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

## <a name="using-offline-deployment"></a><span data-ttu-id="2d425-191">Con la distribuzione Offline</span><span class="sxs-lookup"><span data-stu-id="2d425-191">Using Offline Deployment</span></span>

<span data-ttu-id="2d425-192">In alcuni casi, non è possibile o pratica distribuire le applicazioni e il contenuto in un sito Web IIS da una posizione remota.</span><span class="sxs-lookup"><span data-stu-id="2d425-192">In some cases, it's not possible or practical to deploy applications and content to an IIS website from a remote location.</span></span> <span data-ttu-id="2d425-193">Ad esempio, il computer di origine e destinazione potrebbe essere in reti isolate o segmenti di rete, o criteri del firewall potrebbero non consentire l'accesso remoto.</span><span class="sxs-lookup"><span data-stu-id="2d425-193">For example, the source and destination computers may be in isolated networks or network segments, or firewall policy may not permit remote access.</span></span>

<span data-ttu-id="2d425-194">In questi scenari, è comunque possibile usare la creazione di pacchetti e la pubblicazione delle funzionalità di distribuzione Web; è sufficiente non è possibile usarli da una posizione remota.</span><span class="sxs-lookup"><span data-stu-id="2d425-194">In scenarios like these, you can still use the packaging and publishing capabilities of Web Deploy; you just can't use them from a remote location.</span></span> <span data-ttu-id="2d425-195">Al contrario, un amministratore nel server di destinazione deve copiare il pacchetto web nel server e importarlo tramite Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="2d425-195">Instead, an administrator on the destination server must copy the web package onto the server and import it through IIS Manager.</span></span>

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

<span data-ttu-id="2d425-196">L'approccio di distribuzione non in linea è in genere utile in ambienti di produzione con connessione Internet, in cui i server in una rete perimetrale possono dispone di connettività limitata con i computer nella rete interna.</span><span class="sxs-lookup"><span data-stu-id="2d425-196">The offline deployment approach is typically useful in Internet-facing production environments, where servers in a perimeter network may have restricted connectivity with computers in the internal network.</span></span>

<span data-ttu-id="2d425-197">Per un esempio end-to-end di uno scenario che usa l'approccio di distribuzione non in linea, vedere [Scenario: Configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2d425-197">For an end-to-end example of a scenario that uses the offline deployment approach, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

## <a name="further-reading"></a><span data-ttu-id="2d425-198">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="2d425-198">Further Reading</span></span>

<span data-ttu-id="2d425-199">Per ulteriori informazioni sulla sintassi e le operazioni da riga di comando di distribuzione Web, vedere [riferimento della riga di comando di distribuzione Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-199">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="2d425-200">Per altre informazioni sull'uso di *. deploy. cmd* del file, vedere [come: Installare un pacchetto di distribuzione usando il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-200">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>

<span data-ttu-id="2d425-201">Per istruzioni generali sui diversi modi in cui è possibile distribuire i pacchetti web da un computer remoto, vedere [tramite distribuzione Web in remoto](https://technet.microsoft.com/library/ee461175(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-201">For more general guidance on the different ways in which you can deploy web packages from a remote computer, see [Using Web Deploy Remotely](https://technet.microsoft.com/library/ee461175(WS.10).aspx).</span></span> <span data-ttu-id="2d425-202">Per altre informazioni sull'uso di distribuzione Web su richiesta, vedere [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2d425-202">For more information on using Web Deploy On Demand, see [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d425-203">[Precedente](configuring-server-environments-for-web-deployment.md)
> [Successivo](scenario-configuring-a-test-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="2d425-203">[Previous](configuring-server-environments-for-web-deployment.md)
[Next](scenario-configuring-a-test-environment-for-web-deployment.md)</span></span>