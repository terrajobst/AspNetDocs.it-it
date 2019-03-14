---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Distribuzione Web ASP.NET tramite Visual Studio: Impostazione delle autorizzazioni di cartella | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 930f46c0ddb0b77525098291393e526107a542d2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054468"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a><span data-ttu-id="0d68a-103">Distribuzione Web ASP.NET tramite Visual Studio: Impostazione delle autorizzazioni delle cartelle</span><span class="sxs-lookup"><span data-stu-id="0d68a-103">ASP.NET Web Deployment using Visual Studio: Setting Folder Permissions</span></span>
====================
<span data-ttu-id="0d68a-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0d68a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0d68a-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="0d68a-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="0d68a-106">Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="0d68a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="0d68a-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d68a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="0d68a-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0d68a-108">Overview</span></span>

<span data-ttu-id="0d68a-109">In questa esercitazione, impostare le autorizzazioni per il *Elmah* cartella web distribuito in modo che l'applicazione può creare i file di log nella cartella del sito.</span><span class="sxs-lookup"><span data-stu-id="0d68a-109">In this tutorial, you set folder permissions for the *Elmah* folder in the deployed web site so that the application can create log files in that folder.</span></span>

<span data-ttu-id="0d68a-110">Quando si testa un'applicazione web in Visual Studio usando il Visual Studio Development Server (Cassini) o IIS Express, l'applicazione viene eseguita con la tua identità.</span><span class="sxs-lookup"><span data-stu-id="0d68a-110">When you test a web application in Visual Studio using the Visual Studio Development Server (Cassini) or IIS Express, the application runs under your identity.</span></span> <span data-ttu-id="0d68a-111">Si è molto probabile che un amministratore nel computer di sviluppo e disporre di autorità completa eseguire alcuna azione per qualsiasi file in una cartella specifica.</span><span class="sxs-lookup"><span data-stu-id="0d68a-111">You are most likely an administrator on your development computer and have full authority to do anything to any file in any folder.</span></span> <span data-ttu-id="0d68a-112">Ma quando si esegue un'applicazione in IIS, può essere eseguito con l'identità definita per il pool di applicazioni assegnato al sito.</span><span class="sxs-lookup"><span data-stu-id="0d68a-112">But when an application runs under IIS, it runs under the identity defined for the application pool that the site is assigned to.</span></span> <span data-ttu-id="0d68a-113">Si tratta in genere di un account definito dal sistema che dispone di autorizzazioni limitate.</span><span class="sxs-lookup"><span data-stu-id="0d68a-113">This is typically a system-defined account that has limited permissions.</span></span> <span data-ttu-id="0d68a-114">Per impostazione predefinita dispone di autorizzazioni read ed execute sul file e cartelle dell'applicazione web, ma non ha accesso in scrittura.</span><span class="sxs-lookup"><span data-stu-id="0d68a-114">By default it has read and execute permissions on your web application's files and folders, but it doesn't have write access.</span></span>

<span data-ttu-id="0d68a-115">Questo diventa un problema se l'applicazione crea o aggiorna i file, che è un comune necessarie nelle applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="0d68a-115">This becomes an issue if your application creates or updates files, which is a common need in web applications.</span></span> <span data-ttu-id="0d68a-116">Nell'applicazione di Contoso University, Elmah crea i file XML nel *Elmah* cartella per salvare i dettagli sugli errori.</span><span class="sxs-lookup"><span data-stu-id="0d68a-116">In the Contoso University application, Elmah creates XML files in the *Elmah* folder in order to save details about errors.</span></span> <span data-ttu-id="0d68a-117">Anche se non si usa un elemento, ad esempio Elmah, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre attività di scrittura dei dati in una cartella nel sito.</span><span class="sxs-lookup"><span data-stu-id="0d68a-117">Even if you don't use something like Elmah, your site might let users upload files or perform other tasks that write data to a folder in your site.</span></span>

<span data-ttu-id="0d68a-118">Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="0d68a-118">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="test-error-logging-and-reporting"></a><span data-ttu-id="0d68a-119">Segnalazione e registrazione degli errori di test</span><span class="sxs-lookup"><span data-stu-id="0d68a-119">Test error logging and reporting</span></span>

<span data-ttu-id="0d68a-120">Per vedere come l'applicazione non funziona correttamente in IIS (anche se ha quando è stata testata in Visual Studio), si può causare un errore che viene in genere registrato da Elmah e quindi aprirla il log degli errori Elmah per visualizzare i dettagli.</span><span class="sxs-lookup"><span data-stu-id="0d68a-120">To see how the application doesn't work correctly in IIS (although it did when you tested it in Visual Studio), you can cause an error that would normally be logged by Elmah, and then open the Elmah error log to see the details.</span></span> <span data-ttu-id="0d68a-121">Se Elmah non è riuscito a creare un file XML e archiviare i dettagli dell'errore, viene visualizzato un report degli errori vuoto.</span><span class="sxs-lookup"><span data-stu-id="0d68a-121">If Elmah was unable to create an XML file and store the error details, you see an empty error report.</span></span>

<span data-ttu-id="0d68a-122">Aprire un browser e passare a `http://localhost/ContosoUniversity`, e quindi richiedere un URL non valido, ad esempio *Studentsxxx.aspx*.</span><span class="sxs-lookup"><span data-stu-id="0d68a-122">Open a browser and go to `http://localhost/ContosoUniversity`, and then request an invalid URL like *Studentsxxx.aspx*.</span></span> <span data-ttu-id="0d68a-123">Verrà visualizzata una pagina di errore generati dal sistema anziché il *GenericErrorPage* pagina perché il `customErrors` impostazione nel file Web. config è "RemoteOnly" ed è in esecuzione IIS in locale:</span><span class="sxs-lookup"><span data-stu-id="0d68a-123">You see a system-generated error page instead of the *GenericErrorPage.aspx* page because the `customErrors` setting in the Web.config file is "RemoteOnly" and you are running IIS locally:</span></span>

![Pagina di errore 404 HTTP](setting-folder-permissions/_static/image1.png)

<span data-ttu-id="0d68a-125">A questo punto eseguire *Elmah.axd* per visualizzare il report di errore.</span><span class="sxs-lookup"><span data-stu-id="0d68a-125">Now run *Elmah.axd* to see the error report.</span></span> <span data-ttu-id="0d68a-126">Dopo l'accesso con le credenziali dell'account amministratore (&quot;admin&quot; e &quot;devpwd&quot;), è possibile visualizzare una pagina di log degli errori vuoto perché non è riuscito a creare un file XML in Elmah il *Elmah*cartella:</span><span class="sxs-lookup"><span data-stu-id="0d68a-126">After you log in with the administrator account credentials (&quot;admin&quot; and &quot;devpwd&quot;), you see an empty error log page because Elmah was unable to create an XML file in the *Elmah* folder:</span></span>

![Errore del log vuoto](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a><span data-ttu-id="0d68a-128">Impostare le autorizzazioni di scrittura sulla cartella Elmah</span><span class="sxs-lookup"><span data-stu-id="0d68a-128">Set write permission on the Elmah folder</span></span>

<span data-ttu-id="0d68a-129">È possibile impostare manualmente le autorizzazioni della cartella oppure è possibile renderlo una parte del processo di distribuzione automatica.</span><span class="sxs-lookup"><span data-stu-id="0d68a-129">You can set folder permissions manually or you can make it an automatic part of the deployment process.</span></span> <span data-ttu-id="0d68a-130">Rendendo automatica richiede codice MSBuild complesso e poiché è sufficiente eseguire questa operazione la prima volta che si distribuisce, i passaggi seguenti illustrano come eseguire l'operazione manualmente.</span><span class="sxs-lookup"><span data-stu-id="0d68a-130">Making it automatic requires complex MSBuild code, and since you only have to do this the first time you deploy, the following steps how to do it manually.</span></span> <span data-ttu-id="0d68a-131">(Per informazioni su come effettuare questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sul blog di Sayed Hashimi.)</span><span class="sxs-lookup"><span data-stu-id="0d68a-131">(For information about how to make this part of the deployment process, see [Setting Folder Permissions on Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) on Sayed Hashimi's blog.)</span></span>

1. <span data-ttu-id="0d68a-132">Nelle **Esplora File**, passare alla *C:\inetpub\wwwroot\ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="0d68a-132">In **File Explorer**, navigate to *C:\inetpub\wwwroot\ContosoUniversity*.</span></span> <span data-ttu-id="0d68a-133">Fare doppio clic sui *Elmah* cartella, selezionare **delle proprietà**e quindi selezionare il **sicurezza** scheda.</span><span class="sxs-lookup"><span data-stu-id="0d68a-133">Right-click the *Elmah* folder, select **Properties**, and then select the **Security** tab.</span></span>
2. <span data-ttu-id="0d68a-134">Fare clic su **Modifica**.</span><span class="sxs-lookup"><span data-stu-id="0d68a-134">Click **Edit**.</span></span>
3. <span data-ttu-id="0d68a-135">Nel **le autorizzazioni per Elmah** finestra di dialogo **DefaultAppPool**e quindi selezionare il **scrivere** casella di controllo la **Consenti** colonna.</span><span class="sxs-lookup"><span data-stu-id="0d68a-135">In the **Permissions for Elmah** dialog box, select **DefaultAppPool**, and then select the **Write** check box in the **Allow** column.</span></span>

    ![Autorizzazioni per la cartella ELMAH](setting-folder-permissions/_static/image3.png)

    <span data-ttu-id="0d68a-137">(Se non viene visualizzata **DefaultAppPool** nel **nomi utente o gruppo** elenco, probabilmente utilizzato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer.</span><span class="sxs-lookup"><span data-stu-id="0d68a-137">(If you don't see **DefaultAppPool** in the **Group or user names** list, you probably used some other method than the one specified in this tutorial to set up IIS and ASP.NET 4 on your computer.</span></span> <span data-ttu-id="0d68a-138">In tal caso, scoprire quali identità viene usata dal pool di applicazioni assegnato all'applicazione Contoso University e concedere l'autorizzazione di scrittura per tale identità.</span><span class="sxs-lookup"><span data-stu-id="0d68a-138">In that case, find out what identity is used by the application pool assigned to the Contoso University application, and grant write permission to that identity.</span></span> <span data-ttu-id="0d68a-139">Vedere i collegamenti relativi a identità del pool di applicazioni alla fine di questa esercitazione). Fare clic su **OK** in entrambe le finestre di dialogo.</span><span class="sxs-lookup"><span data-stu-id="0d68a-139">See the links about application pool identities at the end of this tutorial.) Click **OK** in both dialog boxes.</span></span>

## <a name="retest-error-logging-and-reporting"></a><span data-ttu-id="0d68a-140">Testare nuovamente la registrazione degli errori e creazione di report</span><span class="sxs-lookup"><span data-stu-id="0d68a-140">Retest error logging and reporting</span></span>

<span data-ttu-id="0d68a-141">Per testare questo causa un errore nuovamente nello stesso modo (richiesta di un URL non valido) ed eseguire la **Log degli errori** pagina.</span><span class="sxs-lookup"><span data-stu-id="0d68a-141">Test by causing an error again in the same way (request a bad URL) and run the **Error Log** page.</span></span> <span data-ttu-id="0d68a-142">Questa volta l'errore viene visualizzato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="0d68a-142">This time the error appears on the page.</span></span>

![Pagina di Log degli errori ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a><span data-ttu-id="0d68a-144">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0d68a-144">Summary</span></span>

<span data-ttu-id="0d68a-145">È stata ora completata tutte le attività necessarie per ottenere Contoso University funziona correttamente in IIS nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="0d68a-145">You have now completed all of the tasks necessary to get Contoso University working correctly in IIS on your local computer.</span></span> <span data-ttu-id="0d68a-146">Nella prossima esercitazione, si apporterà del sito disponibili pubblicamente per distribuirla in Azure.</span><span class="sxs-lookup"><span data-stu-id="0d68a-146">In the next tutorial, you will make the site publicly available by deploying it to Azure.</span></span>

## <a name="more-information"></a><span data-ttu-id="0d68a-147">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="0d68a-147">More information</span></span>

<span data-ttu-id="0d68a-148">In questo esempio, il motivo per cui non è riuscito a salvare file di log Elmah è abbastanza ovvio.</span><span class="sxs-lookup"><span data-stu-id="0d68a-148">In this example, the reason why Elmah was unable to save log files was fairly obvious.</span></span> <span data-ttu-id="0d68a-149">È possibile utilizzare la traccia di IIS nei casi in cui non è così ovvia; la causa del problema visualizzare [risoluzione dei problemi Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) sul sito IIS.net.</span><span class="sxs-lookup"><span data-stu-id="0d68a-149">You can use IIS tracing in cases where the cause of the problem is not so obvious; see [Troubleshooting Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) on the IIS.net site.</span></span>

<span data-ttu-id="0d68a-150">Per altre informazioni su come concedere le autorizzazioni per le identità del pool di applicazioni, vedere [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [proteggere contenuti in IIS tramite ACL del File System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) sul sito IIS.net.</span><span class="sxs-lookup"><span data-stu-id="0d68a-150">For more information about how to grant permissions to application pool identities, see [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) and [Secure Content in IIS Through File System ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) on the IIS.net site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0d68a-151">[Precedente](deploying-to-iis.md)
> [Successivo](deploying-to-production.md)</span><span class="sxs-lookup"><span data-stu-id="0d68a-151">[Previous](deploying-to-iis.md)
[Next](deploying-to-production.md)</span></span>