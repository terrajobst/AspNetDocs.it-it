---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1,1 con IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Sebbene Windows Server 2003 includa sia IIS 6,0 che ASP.NET 1,1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper descrive come abilitare IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523310"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="57a38-104">Esecuzione di ASP.NET 1.1 con IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="57a38-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="57a38-105">Sebbene Windows Server 2003 includa sia IIS 6,0 che ASP.NET 1,1, questi componenti sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="57a38-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="57a38-106">In questo white paper viene descritto come abilitare IIS 6,0 e ASP.NET 1,1 e vengono consigliate diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="57a38-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="57a38-107">Si applica a ASP.NET 1,1 e IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="57a38-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="57a38-108">ASP.NET 1,1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6,0.</span><span class="sxs-lookup"><span data-stu-id="57a38-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="57a38-109">IIS 6,0 e ASP.NET 1,1 sono progettati per integrarsi in modo semplice e ASP.NET ora il nuovo modello di processo di lavoro di IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="57a38-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="57a38-110">ASP.NET 1,1 non è installato per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="57a38-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="57a38-111">Diversamente dalle versioni precedenti dei sistemi operativi server Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. e non è ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="57a38-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="57a38-112">Sono disponibili due opzioni per l'abilitazione di IIS:</span><span class="sxs-lookup"><span data-stu-id="57a38-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="57a38-113">Abilitazione di IIS, opzione #1-Configurazione guidata server</span><span class="sxs-lookup"><span data-stu-id="57a38-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="57a38-114">Windows Server 2003 fornisce una nuova configurazione guidata server per semplificare la configurazione del server nella modalità desiderata.</span><span class="sxs-lookup"><span data-stu-id="57a38-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="57a38-115">Per avviare la procedura guidata, si noti che per eseguire la procedura guidata è necessario accedere come amministratore: andare a: Start | Programmi | Strumenti di amministrazione e selezionare "Configura server".</span><span class="sxs-lookup"><span data-stu-id="57a38-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="57a38-116">Una volta selezionata, verrà visualizzata la schermata di apertura della configurazione guidata server:</span><span class="sxs-lookup"><span data-stu-id="57a38-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="57a38-117">Fare clic su' avanti &gt;':</span><span class="sxs-lookup"><span data-stu-id="57a38-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="57a38-118">Fare clic su' avanti &gt;'</span><span class="sxs-lookup"><span data-stu-id="57a38-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="57a38-119">In questa schermata sarà necessario selezionare "server applicazioni (IIS, ASP.NET)" come opzioni da configurare.</span><span class="sxs-lookup"><span data-stu-id="57a38-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="57a38-120">Fare clic su' avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="57a38-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="57a38-121">Dopo aver selezionato per configurare il server come server applicazioni, verrà visualizzata la schermata che richiede le funzionalità aggiuntive da installare.</span><span class="sxs-lookup"><span data-stu-id="57a38-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="57a38-122">Per impostazione predefinita, nessuna delle due opzioni è selezionata.</span><span class="sxs-lookup"><span data-stu-id="57a38-122">Neither option is selected by default.</span></span> <span data-ttu-id="57a38-123">Per abilitare automaticamente ASP.NET, è necessario selezionare ' Abilita ASP. "NET".</span><span class="sxs-lookup"><span data-stu-id="57a38-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="57a38-124">Fare clic su' avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="57a38-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="57a38-125">In questa schermata vengono visualizzate le opzioni da installare.</span><span class="sxs-lookup"><span data-stu-id="57a38-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="57a38-126">Fare clic su' avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="57a38-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="57a38-127">Questa schermata viene visualizzata durante l'installazione delle opzioni selezionate.</span><span class="sxs-lookup"><span data-stu-id="57a38-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="57a38-128">È normale visualizzare le altre finestre di dialogo visualizzate durante l'installazione dei servizi.</span><span class="sxs-lookup"><span data-stu-id="57a38-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="57a38-129">È inoltre possibile che venga richiesto il percorso del CD di installazione di Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="57a38-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="57a38-130">Al termine, fare clic su' avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="57a38-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="57a38-131">Fare clic su' fine ': Windows Server 2003 è ora configurato per supportare IIS 6,0 e ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="57a38-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="57a38-132">Abilitazione di IIS, opzione #2-Configurazione manuale di IIS e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="57a38-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="57a38-133">Se non si desidera utilizzare là Configurazione guidata server ', è possibile installare facoltativamente IIS 6,0 e ASP.NET 1,1 utilizzando ' installazione applicazioni ' dal pannello di controllo.</span><span class="sxs-lookup"><span data-stu-id="57a38-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="57a38-134">Aprire prima il pannello di controllo:</span><span class="sxs-lookup"><span data-stu-id="57a38-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="57a38-135">Quindi, fare clic su "Aggiungi/Rimuovi componenti di Windows" per aprire la "procedura guidata componenti di Windows":</span><span class="sxs-lookup"><span data-stu-id="57a38-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="57a38-136">Evidenziare e selezionare "server applicazioni", quindi fare clic su "dettagli".</span><span class="sxs-lookup"><span data-stu-id="57a38-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="57a38-137">pulsante</span><span class="sxs-lookup"><span data-stu-id="57a38-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="57a38-138">Per installare ASP.NET, selezionare ' ASP. "NET".</span><span class="sxs-lookup"><span data-stu-id="57a38-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="57a38-139">Fare clic su' OK ' per tornare alla procedura guidata componenti di Windows.</span><span class="sxs-lookup"><span data-stu-id="57a38-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="57a38-140">Fare clic su' avanti &gt;' dalla procedura guidata componenti di Windows per avviare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="57a38-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="57a38-141">È normale visualizzare le altre finestre di dialogo visualizzate durante l'installazione dei servizi.</span><span class="sxs-lookup"><span data-stu-id="57a38-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="57a38-142">È inoltre possibile che venga richiesto il percorso del CD di installazione di Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="57a38-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="57a38-143">Al termine dell'installazione, verrà visualizzata l'ultima schermata della creazione guidata componente di Windows:</span><span class="sxs-lookup"><span data-stu-id="57a38-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="57a38-144">IIS 6,0 e ASP.NET 1,1 sono ora configurati e disponibili.</span><span class="sxs-lookup"><span data-stu-id="57a38-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="57a38-145">Impostazioni consigliate</span><span class="sxs-lookup"><span data-stu-id="57a38-145">Recommended Settings</span></span>

<span data-ttu-id="57a38-146">Quando si esegue ASP.NET 1,1 con IIS 6,0 sono disponibili diverse impostazioni di configurazione consigliate per ottenere prestazioni ottimali da ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="57a38-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="57a38-147">Configurazione dei limiti di memoria del processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="57a38-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="57a38-148">Configurazione del riciclo del processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="57a38-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="57a38-149">Configurazione dei limiti di memoria del processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="57a38-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="57a38-150">Per impostazione predefinita, IIS 6,0 non imposta un limite per la quantità di memoria che IIS può utilizzare.</span><span class="sxs-lookup"><span data-stu-id="57a38-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="57a38-151">ASP. La funzionalità cache di NET si basa su una limitazione della memoria in modo che la cache possa rimuovere in modo proattivo gli elementi inutilizzati dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="57a38-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="57a38-152">Si consiglia di configurare la funzionalità di riciclo della memoria di IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="57a38-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="57a38-153">Per configurare questo gestore di Internet Information Services aperto (Start | Programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="57a38-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="57a38-154">Una volta aperta, espandere la cartella ' pool di applicazioni ':</span><span class="sxs-lookup"><span data-stu-id="57a38-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="57a38-155">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="57a38-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="57a38-156">Fare clic con il pulsante destro del mouse sul pool di applicazioni, ad esempio ' DefaultAppPool ', quindi selezionare ' proprietà':</span><span class="sxs-lookup"><span data-stu-id="57a38-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="57a38-157">Abilitare quindi il riciclo della memoria facendo clic su' memoria massima usata (in megabyte):'.</span><span class="sxs-lookup"><span data-stu-id="57a38-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="57a38-158">Il valore non deve essere maggiore della quantità di memoria fisica (non virtuale) nel server, una corretta approssimazione è il 60% della memoria fisica, ovvero per un server con 512 MB di memoria fisica selezionare 310.</span><span class="sxs-lookup"><span data-stu-id="57a38-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="57a38-159">È inoltre consigliabile che il valore massimo non superi 800MB quando si utilizza uno spazio degli indirizzi da 2 GB.</span><span class="sxs-lookup"><span data-stu-id="57a38-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="57a38-160">Se lo spazio degli indirizzi di memoria del server è 3GB, il limite massimo di memoria per il processo di lavoro può essere pari a 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="57a38-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="57a38-161">Fare clic su "applica" e su "OK" per uscire dalla finestra di dialogo delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="57a38-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="57a38-162">Ripetere questa operazione per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="57a38-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="57a38-163">Configurazione del riciclo del ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="57a38-163">Configuring worker recycling</span></span>

<span data-ttu-id="57a38-164">Per impostazione predefinita, IIS 6,0 è configurato per riciclare il processo di lavoro ogni 29 ore.</span><span class="sxs-lookup"><span data-stu-id="57a38-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="57a38-165">Si tratta di un po' aggressivo per un'applicazione che esegue ASP.NET ed è consigliabile disabilitare il riciclo automatico dei processi di lavoro.</span><span class="sxs-lookup"><span data-stu-id="57a38-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="57a38-166">Per disabilitare il riciclo automatico del processo di lavoro, aprire prima Internet Information Services Manager (Start | Programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="57a38-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="57a38-167">Una volta aperta, espandere la cartella ' pool di applicazioni ':</span><span class="sxs-lookup"><span data-stu-id="57a38-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="57a38-168">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="57a38-168">For each application pool:</span></span>

1. <span data-ttu-id="57a38-169">Fare clic con il pulsante destro del mouse sul pool di applicazioni, ad esempio ' DefaultAppPool ', quindi selezionare ' proprietà':</span><span class="sxs-lookup"><span data-stu-id="57a38-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="57a38-170">Deseleziona ' riciclo processo di lavoro (in minuti):':</span><span class="sxs-lookup"><span data-stu-id="57a38-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="57a38-171">Fare clic su "applica" e su "OK" per uscire dalla finestra di dialogo delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="57a38-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="57a38-172">Ripetere questa operazione per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="57a38-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="57a38-173">Concessione dell'accesso in scrittura al file system</span><span class="sxs-lookup"><span data-stu-id="57a38-173">Granting write access to the file system</span></span>

<span data-ttu-id="57a38-174">Se l'applicazione richiede l'accesso in scrittura al file system e si utilizza NTFS, sarà necessario modificare un elenco di controllo di accesso (ACL) nella cartella o nel file per concedere l'accesso ASP.NET a.</span><span class="sxs-lookup"><span data-stu-id="57a38-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="57a38-175">Ad esempio, per concedere a ASP.NET l'accesso in scrittura alla c:\Inetpub\Wwwroot prima apertura di Esplora risorse e passare alla directory:</span><span class="sxs-lookup"><span data-stu-id="57a38-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="57a38-176">Fare quindi clic con il pulsante destro del mouse sulla directory, ad esempio "wwwroot" e selezionare Proprietà.</span><span class="sxs-lookup"><span data-stu-id="57a38-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="57a38-177">Quando viene visualizzata la finestra di dialogo Proprietà, selezionare la scheda "sicurezza":</span><span class="sxs-lookup"><span data-stu-id="57a38-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="57a38-178">La directory c:\inetpub\wwwroot\ è una directory speciale in cui lo speciale gruppo IIS 6,0' IIS\_WPG ' ha già concesso la lettura &amp; eseguire, elencare il contenuto della cartella e le autorizzazioni di lettura.</span><span class="sxs-lookup"><span data-stu-id="57a38-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="57a38-179">Tuttavia, per concedere l'autorizzazione di scrittura, è necessario fare clic sulla casella di controllo Consenti per la scrittura:</span><span class="sxs-lookup"><span data-stu-id="57a38-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="57a38-180">IIS 6,0 ora dispone dell'autorizzazione di scrittura per questa cartella.</span><span class="sxs-lookup"><span data-stu-id="57a38-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="57a38-181">Per concedere autorizzazioni di scrittura per altre cartelle, attenersi alla seguente procedura: si noti che potrebbe essere necessario aggiungere il gruppo di IIS\_WPG se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="57a38-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="57a38-182">La concessione dell'autorizzazione di scrittura per IIS\_WPG consentirà a qualsiasi applicazione ASP.NET di scrivere in questa directory.</span><span class="sxs-lookup"><span data-stu-id="57a38-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="57a38-183">Supporto dell'autenticazione integrata con SQL Server</span><span class="sxs-lookup"><span data-stu-id="57a38-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="57a38-184">L'autenticazione integrata consente SQL Server di utilizzare l'autenticazione di Windows NT per convalidare gli account di accesso SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57a38-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="57a38-185">Questo consente all'utente di ignorare il processo di accesso SQL Server standard.</span><span class="sxs-lookup"><span data-stu-id="57a38-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="57a38-186">Con questo approccio, un utente di rete può accedere a un database di SQL Server senza specificare una password o un identificatore di accesso separato perché SQL Server Ottiene le informazioni sull'utente e sulla password dal processo di sicurezza di rete di Windows NT.</span><span class="sxs-lookup"><span data-stu-id="57a38-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="57a38-187">La scelta dell'autenticazione integrata per le applicazioni ASP.NET è una scelta ottimale perché non vengono mai archiviate credenziali nella stringa di connessione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="57a38-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="57a38-188">Piuttosto la stringa di connessione usata per connettersi a SQL sarà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="57a38-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="57a38-189">Questa stringa di connessione indica SQL Server di utilizzare le credenziali di Windows dell'applicazione che tenta di accedere a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57a38-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="57a38-190">Nel caso di ASP.NET/IIS 6, si tratta di un account nel gruppo di IIS\_WPG.</span><span class="sxs-lookup"><span data-stu-id="57a38-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="57a38-191">Per abilitare l'autenticazione integrata tra SQL Server e ASP.NET, è necessario prima di tutto assicurarsi che SQL Server sia configurato per l'autenticazione integrata o per l'autenticazione in modalità mista. verificare con l'amministratore di database per determinare questo problema.</span><span class="sxs-lookup"><span data-stu-id="57a38-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="57a38-192">Se SQL Server è in una di queste due modalità, è possibile usare l'autenticazione integrata.</span><span class="sxs-lookup"><span data-stu-id="57a38-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="57a38-193">Aprire SQL Server Enterprise Manager (Start | Programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:</span><span class="sxs-lookup"><span data-stu-id="57a38-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="57a38-194">Se il gruppo ' BUILTINT\IIS\_WPG ' non è elencato, fare clic con il pulsante destro del mouse su account di accesso e selezionare ' nuovo login:</span><span class="sxs-lookup"><span data-stu-id="57a38-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="57a38-195">Nella casella di testo "Name:" immettere "[Server/nome dominio] \IIS\_WPG" oppure fare clic sul pulsante con i puntini di sospensione per aprire il selettore utenti/gruppi di Windows NT:</span><span class="sxs-lookup"><span data-stu-id="57a38-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="57a38-196">Selezionare il gruppo di IIS\_WPG del computer corrente, fare clic su' Aggiungi ' e OK per chiudere la selezione.</span><span class="sxs-lookup"><span data-stu-id="57a38-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="57a38-197">Sarà quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="57a38-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="57a38-198">Per impostare il database predefinito, scegliere dall'elenco a discesa, ad esempio, di seguito è selezionato Northwind:</span><span class="sxs-lookup"><span data-stu-id="57a38-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="57a38-199">Fare quindi clic sulla scheda accesso al database:</span><span class="sxs-lookup"><span data-stu-id="57a38-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="57a38-200">Fare clic sulla casella di controllo Consenti per ogni database a cui si desidera consentire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="57a38-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="57a38-201">È inoltre necessario selezionare i ruoli del database, controllando il proprietario del database\_assicurerà che l'account di accesso disponga di tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.</span><span class="sxs-lookup"><span data-stu-id="57a38-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="57a38-202">Fare clic su OK per uscire dalla finestra di dialogo delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="57a38-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="57a38-203">L'applicazione ASP.NET è ora configurata per supportare l'autenticazione integrata del SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57a38-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="57a38-204">Non eseguire ASP.NET 1,0 in modalità nativa di IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="57a38-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="57a38-205">ASP.NET 1,0 in IIS 6,0 è supportato solo in modalità di compatibilità con IIS 5.</span><span class="sxs-lookup"><span data-stu-id="57a38-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="57a38-206">Per configurare ASP.NET 1,0 per l'esecuzione in modalità di compatibilità IIS 5,0, aprire Gestione servizi Internet e fare clic con il pulsante destro del mouse su siti Web e scegliere Proprietà:</span><span class="sxs-lookup"><span data-stu-id="57a38-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="57a38-207">Passare alla scheda servizio e selezionare? Eseguire il servizio WWW in modalità di isolamento IIS 5,0?:</span><span class="sxs-lookup"><span data-stu-id="57a38-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
