---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626773"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="0aa23-103">Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice</span><span class="sxs-lookup"><span data-stu-id="0aa23-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>

<span data-ttu-id="0aa23-104">di [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0aa23-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0aa23-105">Scarica progetto Starter</span><span class="sxs-lookup"><span data-stu-id="0aa23-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="0aa23-106">Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="0aa23-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="0aa23-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0aa23-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="0aa23-108">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="0aa23-108">Overview</span></span>

<span data-ttu-id="0aa23-109">Dopo la distribuzione iniziale, il lavoro di manutenzione e sviluppo del sito Web continua e, prima di lungo, sarà necessario distribuire un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="0aa23-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="0aa23-110">Questa esercitazione illustra il processo di distribuzione di un aggiornamento nel codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="0aa23-111">L'aggiornamento implementato e distribuito in questa esercitazione non implica una modifica del database. verranno visualizzate le differenze relative alla distribuzione di una modifica del database nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="0aa23-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="0aa23-112">Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="0aa23-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="0aa23-113">Apportare una modifica al codice</span><span class="sxs-lookup"><span data-stu-id="0aa23-113">Make a code change</span></span>

<span data-ttu-id="0aa23-114">Come semplice esempio di aggiornamento dell'applicazione, si aggiungerà alla pagina degli **insegnanti** un elenco di corsi insegnato dall'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="0aa23-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="0aa23-115">Se si esegue la pagina **Instructors (insegnanti** ), si noterà che nella griglia sono presenti collegamenti **selezionati** , ma che non eseguono alcuna operazione oltre a rendere grigio lo sfondo della riga.</span><span class="sxs-lookup"><span data-stu-id="0aa23-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Pagina degli insegnanti con selezione](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="0aa23-117">A questo punto si aggiungerà il codice che viene eseguito quando si fa clic sul collegamento **Seleziona** e viene visualizzato un elenco dei corsi insegnati dall'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="0aa23-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="0aa23-118">In *Instructors. aspx*aggiungere il markup seguente subito dopo il controllo `Label` **ErrorMessageLabel** :</span><span class="sxs-lookup"><span data-stu-id="0aa23-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="0aa23-119">Eseguire la pagina e selezionare un insegnante.</span><span class="sxs-lookup"><span data-stu-id="0aa23-119">Run the page and select an instructor.</span></span> <span data-ttu-id="0aa23-120">Viene visualizzato un elenco dei corsi tenuti da tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="0aa23-120">You see a list of courses taught by that instructor.</span></span>

    ![Pagina relativa agli insegnanti con corsi insegnati](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="0aa23-122">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="0aa23-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="0aa23-123">Distribuire l'aggiornamento del codice nell'ambiente di test</span><span class="sxs-lookup"><span data-stu-id="0aa23-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="0aa23-124">Prima di poter usare i profili di pubblicazione per eseguire la distribuzione in test, gestione temporanea e produzione, è necessario modificare le opzioni di pubblicazione del database.</span><span class="sxs-lookup"><span data-stu-id="0aa23-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="0aa23-125">Non è più necessario eseguire gli script di concessione e distribuzione dei dati per il database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="0aa23-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="0aa23-126">Aprire la procedura guidata **Pubblica sito Web** facendo clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliendo **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="0aa23-127">Fare clic sul profilo **test** nell'elenco a discesa **profilo** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="0aa23-128">Fare clic sulla scheda **Impostazioni** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="0aa23-129">In **DefaultConnection** nella sezione **database** deselezionare la casella di controllo **Aggiorna database** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="0aa23-130">Fare clic sulla scheda **profilo** , quindi fare clic sul profilo di **gestione temporanea** nell'elenco a discesa **profilo** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="0aa23-131">Quando viene chiesto se si desidera salvare le modifiche apportate al profilo di **test** , fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="0aa23-132">Apportare la stessa modifica nel profilo di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="0aa23-133">Ripetere il processo per apportare la stessa modifica nel profilo di produzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="0aa23-134">Chiudere la procedura guidata **Pubblica sito Web** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="0aa23-135">La distribuzione nell'ambiente di test è ora una semplice operazione di esecuzione di un solo clic di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="0aa23-136">Per rendere più rapido questo processo, è possibile usare la barra degli strumenti **pubblica con un clic** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="0aa23-137">Scegliere **barre degli strumenti** dal menu **Visualizza** e quindi **fare clic sul pulsante pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="0aa23-139">In **Esplora soluzioni**selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="0aa23-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="0aa23-140">sul **Web fare clic su pubblica** barra degli strumenti, scegliere il profilo di pubblicazione del **test** , quindi fare clic su **Pubblica Web** (icona con frecce che puntano a sinistra e a destra).</span><span class="sxs-lookup"><span data-stu-id="0aa23-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="0aa23-142">Visual Studio distribuisce l'applicazione aggiornata e il browser si apre automaticamente al home page.</span><span class="sxs-lookup"><span data-stu-id="0aa23-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="0aa23-143">Eseguire la pagina insegnanti e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="0aa23-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="0aa23-144">Normalmente si eseguono anche test di regressione, ovvero si testa il resto del sito per assicurarsi che la nuova modifica non interrompa le funzionalità esistenti.</span><span class="sxs-lookup"><span data-stu-id="0aa23-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="0aa23-145">Tuttavia, per questa esercitazione si ignorerà questo passaggio e si procederà con la distribuzione dell'aggiornamento alla gestione temporanea e alla produzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="0aa23-146">Quando si esegue la ridistribuzione, Distribuzione Web determina automaticamente quali file sono stati modificati e copia solo i file modificati nel server.</span><span class="sxs-lookup"><span data-stu-id="0aa23-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="0aa23-147">Per impostazione predefinita, Distribuzione Web usa le date delle ultime modifiche nei file per determinare quali sono state modificate.</span><span class="sxs-lookup"><span data-stu-id="0aa23-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="0aa23-148">Alcuni sistemi di controllo del codice sorgente cambiano le date dei file anche se non si modifica il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="0aa23-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="0aa23-149">In tal caso, potrebbe essere necessario configurare Distribuzione Web per utilizzare i checksum dei file per determinare quali file sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="0aa23-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="0aa23-150">Per altre informazioni, vedere [perché tutti i file vengono ridistribuiti anche se non sono stati modificati?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nelle domande frequenti sulla distribuzione di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0aa23-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="0aa23-151">Portare offline l'applicazione durante la distribuzione</span><span class="sxs-lookup"><span data-stu-id="0aa23-151">Take the application offline during deployment</span></span>

<span data-ttu-id="0aa23-152">La modifica che si sta distribuendo ora è una semplice modifica a una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="0aa23-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="0aa23-153">Tuttavia, a volte si distribuiscono modifiche di dimensioni maggiori o si distribuiscono modifiche al codice e al database e il sito potrebbe non funzionare correttamente se un utente richiede una pagina prima del completamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="0aa23-154">Per impedire agli utenti di accedere al sito mentre è in corso la distribuzione, è possibile usare un' *app\_file offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="0aa23-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="0aa23-155">Quando si inserisce un file denominato *app\_offline. htm* nella cartella radice dell'applicazione, in IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="0aa23-156">Per impedire l'accesso durante la distribuzione, inserire l' *app\_offline. htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere l' *app\_offline. htm* dopo la corretta distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="0aa23-157">È possibile configurare Distribuzione Web per inserire automaticamente un' *app predefinita\_file offline. htm* nel server al momento dell'avvio della distribuzione e della relativa rimozione al termine della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="0aa23-158">A tale scopo, è necessario aggiungere l'elemento XML seguente al file del profilo di pubblicazione (con estensione pubxml):</span><span class="sxs-lookup"><span data-stu-id="0aa23-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="0aa23-159">Per questa esercitazione si vedrà come creare e usare un' *app personalizzata\_file offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="0aa23-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="0aa23-160">L'uso di *app\_offline. htm* nel sito di gestione temporanea non è obbligatorio perché non sono disponibili utenti che accedono al sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="0aa23-161">Tuttavia, è consigliabile usare la gestione temporanea per testare tutte le modalità di distribuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-app_offlinehtm"></a><span data-ttu-id="0aa23-162">Crea app\_offline. htm</span><span class="sxs-lookup"><span data-stu-id="0aa23-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="0aa23-163">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="0aa23-164">Creare una **pagina HTML** denominata *app\_offline. htm* (eliminare la "l" finale nell'estensione *HTML* creata da Visual Studio per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="0aa23-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="0aa23-165">Sostituire il markup del modello con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="0aa23-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="0aa23-166">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="0aa23-166">Save and close the file.</span></span>

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="0aa23-167">Copiare l'app\_offline. htm nella cartella radice del sito Web</span><span class="sxs-lookup"><span data-stu-id="0aa23-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="0aa23-168">È possibile utilizzare qualsiasi strumento FTP per copiare i file nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="0aa23-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="0aa23-169">[FileZilla](http://filezilla-project.org/) è uno strumento FTP molto diffuso e viene visualizzato nelle schermate.</span><span class="sxs-lookup"><span data-stu-id="0aa23-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="0aa23-170">Per usare uno strumento FTP, sono necessari tre elementi: l'URL FTP, il nome utente e la password.</span><span class="sxs-lookup"><span data-stu-id="0aa23-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="0aa23-171">L'URL viene visualizzato nella pagina dashboard del sito Web nel portale di gestione di Azure e il nome utente e la password per FTP sono reperibili nel file con *estensione publishsettings* scaricato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="0aa23-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="0aa23-172">Nei passaggi seguenti viene illustrato come ottenere questi valori.</span><span class="sxs-lookup"><span data-stu-id="0aa23-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="0aa23-173">Nel portale di gestione di Azure fare clic sulla scheda **siti Web** , quindi fare clic sul sito Web di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="0aa23-174">Nella pagina **Dashboard** scorrere verso il basso fino a trovare il nome host FTP nella sezione **Quick Glance** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="0aa23-176">Aprire il file staging *. publishsettings* nel blocco note o in un altro editor di testo.</span><span class="sxs-lookup"><span data-stu-id="0aa23-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="0aa23-177">Trovare l'elemento `publishProfile` per il profilo FTP.</span><span class="sxs-lookup"><span data-stu-id="0aa23-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="0aa23-178">Copiare i valori `userName` e `userPWD`.</span><span class="sxs-lookup"><span data-stu-id="0aa23-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="0aa23-180">Aprire lo strumento FTP e accedere all'URL FTP.</span><span class="sxs-lookup"><span data-stu-id="0aa23-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="0aa23-181">Copiare l' *app\_offline. htm* dalla cartella della soluzione alla cartella */site/wwwroot* nel sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copia app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="0aa23-183">Passare all'URL del sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="0aa23-184">Si noterà che viene visualizzata la pagina *app\_offline. htm* invece della Home page.</span><span class="sxs-lookup"><span data-stu-id="0aa23-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![app_offline. htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="0aa23-186">A questo punto si è pronti per la distribuzione in staging.</span><span class="sxs-lookup"><span data-stu-id="0aa23-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="0aa23-187">Distribuire l'aggiornamento del codice per la gestione temporanea e la produzione</span><span class="sxs-lookup"><span data-stu-id="0aa23-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="0aa23-188">Nella barra degli strumenti **pubblica sul Web fare clic** sul profilo di pubblicazione di **staging** , quindi scegliere **Pubblica sito Web**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="0aa23-189">Visual Studio distribuisce l'applicazione aggiornata e apre il browser all'home page del sito.</span><span class="sxs-lookup"><span data-stu-id="0aa23-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="0aa23-190">Viene visualizzata l' *app\_file offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="0aa23-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="0aa23-191">Prima di poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere l' *app\_file offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="0aa23-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="0aa23-192">Tornare allo strumento FTP ed eliminare l' **app\_offline. htm** dal sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="0aa23-193">Nel browser aprire la pagina insegnanti nel sito di gestione temporanea e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="0aa23-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="0aa23-194">Seguire la stessa procedura per la produzione come per la gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="0aa23-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="0aa23-195">Revisione delle modifiche e distribuzione di file specifici</span><span class="sxs-lookup"><span data-stu-id="0aa23-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="0aa23-196">Visual Studio 2012 offre inoltre la possibilità di distribuire singoli file.</span><span class="sxs-lookup"><span data-stu-id="0aa23-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="0aa23-197">Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione oppure copiare il file dall'ambiente di destinazione al progetto locale.</span><span class="sxs-lookup"><span data-stu-id="0aa23-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="0aa23-198">In questa sezione dell'esercitazione si vedrà come usare queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="0aa23-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="0aa23-199">Apportare una modifica a deploy</span><span class="sxs-lookup"><span data-stu-id="0aa23-199">Make a change to deploy</span></span>

1. <span data-ttu-id="0aa23-200">Aprire *content/site. CSS*e trovare il blocco per il tag `body`.</span><span class="sxs-lookup"><span data-stu-id="0aa23-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="0aa23-201">Modificare il valore di `background-color` da `#fff` a `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="0aa23-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="0aa23-202">Visualizzare la modifica nella finestra di anteprima di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="0aa23-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="0aa23-203">Quando si utilizza la procedura guidata **Pubblica sito Web** per pubblicare il progetto, è possibile visualizzare le modifiche che verranno pubblicate facendo doppio clic sul file nella finestra di **Anteprima** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="0aa23-204">Fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="0aa23-205">Dall'elenco a discesa **profilo** selezionare il profilo di pubblicazione del **test** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="0aa23-206">Fare clic su **Anteprima**, quindi fare clic su **Avvia anteprima**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="0aa23-207">Nel riquadro di **Anteprima** , fare doppio clic su **site. CSS**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Fare doppio clic su site. CSS](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="0aa23-209">La finestra di dialogo **Anteprima modifiche** Mostra un'anteprima delle modifiche che verranno distribuite.</span><span class="sxs-lookup"><span data-stu-id="0aa23-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Visualizzare in anteprima le modifiche apportate a site. CSS](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="0aa23-211">Se si fa doppio clic sul file *Web. config* , la finestra di dialogo **Anteprima modifiche** Mostra l'effetto delle trasformazioni di configurazione della build e delle trasformazioni del profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="0aa23-212">A questo punto non è stata eseguita alcuna operazione che comporterebbe la modifica del file *Web. config* nel server, pertanto si prevede di non visualizzare alcuna modifica.</span><span class="sxs-lookup"><span data-stu-id="0aa23-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="0aa23-213">Tuttavia, la finestra **Anteprima modifiche** Mostra erroneamente due modifiche.</span><span class="sxs-lookup"><span data-stu-id="0aa23-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="0aa23-214">Verranno rimossi due elementi XML.</span><span class="sxs-lookup"><span data-stu-id="0aa23-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="0aa23-215">Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **esegui migrazioni Code First all'avvio dell'applicazione** per una classe del contesto di Code First.</span><span class="sxs-lookup"><span data-stu-id="0aa23-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="0aa23-216">Il confronto viene eseguito prima che il processo di pubblicazione aggiunga tali elementi, quindi sembra che vengano rimossi anche se non verranno rimossi.</span><span class="sxs-lookup"><span data-stu-id="0aa23-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="0aa23-217">Questo errore verrà corretto in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="0aa23-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="0aa23-218">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-218">Click **Close**.</span></span>
6. <span data-ttu-id="0aa23-219">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-219">Click **Publish**.</span></span>
7. <span data-ttu-id="0aa23-220">Quando si apre il browser alla home page del sito di test, premere CTRL + F5 per eseguire un aggiornamento hardware per vedere l'effetto della modifica CSS.</span><span class="sxs-lookup"><span data-stu-id="0aa23-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="0aa23-222">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="0aa23-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="0aa23-223">Pubblica file specifici da Esplora soluzioni</span><span class="sxs-lookup"><span data-stu-id="0aa23-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="0aa23-224">Si supponga di non avere lo sfondo blu e di voler ripristinare il colore originale.</span><span class="sxs-lookup"><span data-stu-id="0aa23-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="0aa23-225">In questa sezione verranno ripristinate le impostazioni originali pubblicando un file specifico direttamente da **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="0aa23-226">Aprire *content/site. CSS* e ripristinare l'impostazione di `background-color` su `#fff`.</span><span class="sxs-lookup"><span data-stu-id="0aa23-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="0aa23-227">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul file *content/site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="0aa23-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="0aa23-228">Nel menu di scelta rapida vengono visualizzate tre opzioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-228">The context menu shows three publish options.</span></span>

    ![Opzioni di pubblicazione da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="0aa23-230">Fare clic su **Anteprima modifiche a site. CSS**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="0aa23-231">Viene visualizzata una finestra in cui sono illustrate le differenze tra il file locale e la versione dell'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0aa23-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="0aa23-233">In **Esplora soluzioni**fare nuovamente clic con il pulsante destro del mouse su **site. CSS** e scegliere **Pubblica sito. CSS**.</span><span class="sxs-lookup"><span data-stu-id="0aa23-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="0aa23-234">La finestra **attività di pubblicazione Web** indica che il file è stato pubblicato.</span><span class="sxs-lookup"><span data-stu-id="0aa23-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Finestra attività di pubblicazione Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="0aa23-236">Aprire un browser all'URL `http://localhost/contosouniversity`, quindi premere CTRL + F5 per eseguire un aggiornamento rigido per visualizzare l'effetto della modifica CSS.</span><span class="sxs-lookup"><span data-stu-id="0aa23-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="0aa23-238">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="0aa23-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="0aa23-239">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0aa23-239">Summary</span></span>

<span data-ttu-id="0aa23-240">A questo punto sono stati illustrati diversi modi per distribuire un aggiornamento di un'applicazione che non implica una modifica al database e si è visto come visualizzare in anteprima le modifiche per verificare che ciò che verrà aggiornato è quello previsto.</span><span class="sxs-lookup"><span data-stu-id="0aa23-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="0aa23-241">La pagina relativa agli insegnanti dispone ora di una sezione **corsi didattici** .</span><span class="sxs-lookup"><span data-stu-id="0aa23-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Pagina relativa agli insegnanti con corsi insegnati](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="0aa23-243">Nell'esercitazione successiva viene illustrato come distribuire una modifica del database: verrà aggiunto un campo di data di nascita al database e alla pagina insegnanti.</span><span class="sxs-lookup"><span data-stu-id="0aa23-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0aa23-244">[Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="0aa23-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
