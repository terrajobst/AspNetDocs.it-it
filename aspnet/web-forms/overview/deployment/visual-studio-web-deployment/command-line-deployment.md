---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione da riga di comando | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630921"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="92430-103">Distribuzione Web ASP.NET con Visual Studio: distribuzione da riga di comando</span><span class="sxs-lookup"><span data-stu-id="92430-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="92430-104">di [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="92430-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="92430-105">Scarica progetto Starter</span><span class="sxs-lookup"><span data-stu-id="92430-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="92430-106">Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="92430-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="92430-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="92430-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="92430-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="92430-108">Overview</span></span>

<span data-ttu-id="92430-109">Questa esercitazione illustra come richiamare la pipeline di pubblicazione Web di Visual Studio dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="92430-110">Questa operazione è utile per gli scenari in cui si vuole [automatizzare il processo di distribuzione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anziché eseguirlo manualmente in Visual Studio, in genere usando un [sistema di controllo della versione del codice sorgente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="92430-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="92430-111">Apportare una modifica a deploy</span><span class="sxs-lookup"><span data-stu-id="92430-111">Make a change to deploy</span></span>

<span data-ttu-id="92430-112">Attualmente nella pagina about viene visualizzato il codice del modello.</span><span class="sxs-lookup"><span data-stu-id="92430-112">Currently the About page displays the template code.</span></span>

![Pagina about con codice modello](command-line-deployment/_static/image1.png)

<span data-ttu-id="92430-114">Si sostituirà con il codice che visualizza un riepilogo della registrazione studente.</span><span class="sxs-lookup"><span data-stu-id="92430-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="92430-115">Aprire la pagina *About. aspx* , eliminare tutto il markup all'interno dell'elemento `MainContent` `Content` e inserire il markup seguente al suo posto:</span><span class="sxs-lookup"><span data-stu-id="92430-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="92430-116">Eseguire il progetto e selezionare la pagina **informazioni su** .</span><span class="sxs-lookup"><span data-stu-id="92430-116">Run the project and select the **About** page.</span></span>

![Pagina About (Informazioni)](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="92430-118">Distribuire per eseguire il test tramite la riga di comando</span><span class="sxs-lookup"><span data-stu-id="92430-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="92430-119">Non verrà distribuita un'altra modifica del database. disabilitare quindi la distribuzione del database dbDacFx per il database ASPNET-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="92430-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="92430-120">Aprire la procedura guidata **Pubblica sito Web** e in ognuno dei tre profili di pubblicazione deselezionare la casella di controllo **Aggiorna database** nella scheda **Impostazioni** .</span><span class="sxs-lookup"><span data-stu-id="92430-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="92430-121">Nella pagina iniziale di Windows 8 cercare **prompt dei comandi per gli sviluppatori per VS2012**.</span><span class="sxs-lookup"><span data-stu-id="92430-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="92430-122">Fare clic con il pulsante destro del mouse sull'icona **prompt dei comandi per gli sviluppatori per VS2012** e scegliere **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="92430-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="92430-123">Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file di soluzione:</span><span class="sxs-lookup"><span data-stu-id="92430-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="92430-124">MSBuild compila la soluzione e la distribuisce nell'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="92430-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Output della riga di comando](command-line-deployment/_static/image3.png)

<span data-ttu-id="92430-126">Aprire un browser e passare a `http://localhost/ContosoUniversity`, quindi fare clic sulla pagina **informazioni** per verificare che la distribuzione sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="92430-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="92430-127">Se non sono stati creati studenti in test, verrà visualizzata una pagina vuota sotto l'intestazione **statistiche corpo studente** .</span><span class="sxs-lookup"><span data-stu-id="92430-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="92430-128">Andare alla pagina **studenti** , fare clic su **Aggiungi studente**e aggiungere alcuni studenti, quindi tornare alla pagina **About (informazioni** ) per visualizzare le statistiche degli studenti.</span><span class="sxs-lookup"><span data-stu-id="92430-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Pagina informazioni nell'ambiente di test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="92430-130">Opzioni della riga di comando chiave</span><span class="sxs-lookup"><span data-stu-id="92430-130">Key command line options</span></span>

<span data-ttu-id="92430-131">Il comando immesso passa il percorso del file di soluzione e due proprietà a MSBuild:</span><span class="sxs-lookup"><span data-stu-id="92430-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="92430-132">Distribuzione della soluzione rispetto alla distribuzione di singoli progetti</span><span class="sxs-lookup"><span data-stu-id="92430-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="92430-133">Se si specifica il file di soluzione, verranno compilati tutti i progetti della soluzione.</span><span class="sxs-lookup"><span data-stu-id="92430-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="92430-134">Se si dispone di più progetti Web nella soluzione, si applica il comportamento di MSBuild seguente:</span><span class="sxs-lookup"><span data-stu-id="92430-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="92430-135">Le proprietà specificate nella riga di comando vengono passate a ogni progetto.</span><span class="sxs-lookup"><span data-stu-id="92430-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="92430-136">Pertanto, ogni progetto Web deve disporre di un profilo di pubblicazione con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="92430-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="92430-137">Se si specifica `/p:PublishProfile=Test`, ogni progetto Web deve disporre di un profilo di pubblicazione denominato *test*.</span><span class="sxs-lookup"><span data-stu-id="92430-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="92430-138">È possibile pubblicare correttamente un progetto quando un altro non è ancora in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="92430-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="92430-139">Per ulteriori informazioni, vedere il thread StackOverflow [MSBuild ha esito negativo con due pacchetti](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="92430-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="92430-140">Se si specifica un progetto singolo anziché una soluzione, è necessario aggiungere un parametro che specifichi la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92430-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="92430-141">Se si usa Visual Studio 2012, la riga di comando sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="92430-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="92430-142">Il numero di versione per Visual Studio 2010 è 10,0.</span><span class="sxs-lookup"><span data-stu-id="92430-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="92430-143">Per altre informazioni, vedere [compatibilità dei progetti di Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) nel Blog di Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="92430-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="92430-144">Specifica del profilo di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="92430-144">Specifying the publish profile</span></span>

<span data-ttu-id="92430-145">È possibile specificare il profilo di pubblicazione in base al nome o al percorso completo del file con *estensione pubxml* , come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="92430-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="92430-146">Metodi di pubblicazione Web supportati per la pubblicazione da riga di comando</span><span class="sxs-lookup"><span data-stu-id="92430-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="92430-147">Per la pubblicazione da riga di comando sono supportati tre metodi di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="92430-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="92430-148">`MSDeploy` pubblicare utilizzando Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="92430-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="92430-149">`Package`-pubblicare creando un pacchetto di Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="92430-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="92430-150">È necessario installare il pacchetto separatamente dal comando MSBuild che la crea.</span><span class="sxs-lookup"><span data-stu-id="92430-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="92430-151">`FileSystem`-pubblicare copiando i file in una cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="92430-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="92430-152">Specifica della configurazione e della piattaforma di compilazione</span><span class="sxs-lookup"><span data-stu-id="92430-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="92430-153">La configurazione e la piattaforma di compilazione devono essere impostate in Visual Studio o dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="92430-154">I profili di pubblicazione includono proprietà denominate `LastUsedBuildConfiguration` e `LastUsedPlatform`, ma non è possibile impostare queste proprietà per determinare la modalità di compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="92430-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="92430-155">Per altre informazioni, vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) nel Blog di Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="92430-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="92430-156">Eseguire la distribuzione per lo staging</span><span class="sxs-lookup"><span data-stu-id="92430-156">Deploy to staging</span></span>

<span data-ttu-id="92430-157">Per eseguire la distribuzione in Azure, è necessario aggiungere la password alla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="92430-158">Se la password è stata salvata nel profilo di pubblicazione in Visual Studio, è stata archiviata in formato crittografato nel file con *estensione pubxml. User* .</span><span class="sxs-lookup"><span data-stu-id="92430-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="92430-159">Il file non è accessibile da MSBuild quando si esegue una distribuzione da riga di comando, quindi è necessario passare la password in un parametro della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="92430-160">Copiare la password necessaria dal file con *estensione publishsettings* scaricato in precedenza per il sito Web di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="92430-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="92430-161">La password corrisponde al valore dell'attributo `userPWD` per l'elemento `publishProfile` Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="92430-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Password Distribuzione Web](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="92430-163">Nella pagina iniziale di Windows 8 cercare **prompt dei comandi per gli sviluppatori per VS2012**e fare clic sull'icona per aprire il prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="92430-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="92430-164">Non è necessario aprirlo come amministratore questa volta perché non si esegue la distribuzione in IIS nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="92430-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="92430-165">Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file della soluzione e la password con la password:</span><span class="sxs-lookup"><span data-stu-id="92430-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="92430-166">Si noti che questa riga di comando include un parametro aggiuntivo: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="92430-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="92430-167">Durante la scrittura di questa esercitazione, è necessario impostare la proprietà `AllowUntrustedCertificate` quando si esegue la pubblicazione in Azure dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="92430-168">Quando la correzione per questo bug viene rilasciata, questo parametro non è necessario.</span><span class="sxs-lookup"><span data-stu-id="92430-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="92430-169">Aprire un browser e passare all'URL del sito di gestione temporanea, quindi fare clic sulla pagina **informazioni su** per verificare che la distribuzione sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="92430-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="92430-170">Come illustrato in precedenza per l'ambiente di test, potrebbe essere necessario creare alcuni studenti per visualizzare le statistiche nella pagina **About (informazioni** ).</span><span class="sxs-lookup"><span data-stu-id="92430-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="92430-171">Distribuzione nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="92430-171">Deploy to production</span></span>

<span data-ttu-id="92430-172">Il processo di distribuzione nell'ambiente di produzione è simile al processo di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="92430-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="92430-173">Copiare la password necessaria dal file con *estensione publishsettings* scaricato in precedenza per il sito Web di produzione.</span><span class="sxs-lookup"><span data-stu-id="92430-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="92430-174">Aprire **prompt dei comandi per gli sviluppatori per VS2012**.</span><span class="sxs-lookup"><span data-stu-id="92430-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="92430-175">Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file della soluzione e la password con la password:</span><span class="sxs-lookup"><span data-stu-id="92430-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="92430-176">Per un sito di produzione reale, se si è verificata una modifica del database, in genere si copia l' *app\_file offline. htm* nel sito prima della distribuzione ed eliminarla dopo la corretta distribuzione.</span><span class="sxs-lookup"><span data-stu-id="92430-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="92430-177">Aprire un browser e passare all'URL del sito di gestione temporanea, quindi fare clic sulla pagina **informazioni su** per verificare che la distribuzione sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="92430-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="92430-178">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="92430-178">Summary</span></span>

<span data-ttu-id="92430-179">A questo punto è stato distribuito un aggiornamento dell'applicazione tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="92430-179">You have now deployed an application update by using the command line.</span></span>

![Pagina informazioni nell'ambiente di test](command-line-deployment/_static/image6.png)

<span data-ttu-id="92430-181">Nell'esercitazione successiva verrà visualizzato un esempio di come estendere la pipeline di pubblicazione Web.</span><span class="sxs-lookup"><span data-stu-id="92430-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="92430-182">Nell'esempio viene illustrato come distribuire i file che non sono inclusi nel progetto.</span><span class="sxs-lookup"><span data-stu-id="92430-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92430-183">[Precedente](deploying-a-database-update.md)
> [Successivo](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="92430-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
