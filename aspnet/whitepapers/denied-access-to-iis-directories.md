---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET ha negato l'accesso alle directory IIS | Microsoft Docs
author: rick-anderson
description: In questo white paper vengono descritte le operazioni che è necessario eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory DirectoryName. Non è stato possibile...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638502"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="7cbf2-104">Accesso negato alle directory di IIS per ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7cbf2-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="7cbf2-105">In questo white paper vengono descritte le operazioni che è necessario eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory *DirectoryName* .</span><span class="sxs-lookup"><span data-stu-id="7cbf2-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="7cbf2-106">Non è stato possibile avviare il monitoraggio delle modifiche della directory. "</span><span class="sxs-lookup"><span data-stu-id="7cbf2-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="7cbf2-107">Si applica a ASP.NET 1,0 e ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="7cbf2-108">ASP.NET V1 RTM viene ora eseguito con un account Windows meno privilegiato, registrato come account "ASPNET" in un computer locale.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="7cbf2-109">In alcuni sistemi bloccati, per impostazione predefinita, questo account non dispone dell'accesso in lettura per la sicurezza alle directory di contenuto di un sito Web, alla directory radice dell'applicazione o alla directory radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="7cbf2-110">In questo caso verrà visualizzato l'errore seguente durante la richiesta di pagine da una determinata applicazione Web:</span><span class="sxs-lookup"><span data-stu-id="7cbf2-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="7cbf2-111">Per risolvere questo problema, sarà necessario modificare le autorizzazioni di sicurezza nelle directory appropriate.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="7cbf2-112">In particolare, ASP.NET richiede l'accesso in lettura, esecuzione ed elenco per l'account ASPNET per la radice del sito Web (ad esempio: c:\Inetpub\Wwwroot o qualsiasi directory del sito alternativa configurata in IIS), la directory del contenuto e la directory radice dell'applicazione per monitorare le modifiche del file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="7cbf2-113">La radice dell'applicazione corrisponde al percorso della cartella associato alla directory virtuale dell'applicazione nello strumento di amministrazione di IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="7cbf2-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="7cbf2-114">Si consideri, ad esempio, la seguente gerarchia dell'applicazione nella cartella wwwroot.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="7cbf2-115">Per questo esempio, l'account ASPNET richiede le autorizzazioni di lettura definite in precedenza per il contenuto nella directory MyApp e wwwroot.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="7cbf2-116">Un singolo ACL ereditato nella cartella radice può anche essere usato facoltativamente per entrambe le directory se sono annidate.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="7cbf2-117">Per aggiungere le autorizzazioni a una directory, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="7cbf2-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="7cbf2-118">Utilizzando Esplora risorse, passare alla directory</span><span class="sxs-lookup"><span data-stu-id="7cbf2-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="7cbf2-119">Fare clic con il pulsante destro del mouse sulla cartella directory e scegliere "proprietà"</span><span class="sxs-lookup"><span data-stu-id="7cbf2-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="7cbf2-120">Passare alla scheda "sicurezza" nella finestra di dialogo delle proprietà</span><span class="sxs-lookup"><span data-stu-id="7cbf2-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="7cbf2-121">Fare clic sul pulsante "Aggiungi" e immettere il nome del computer seguito dal nome dell'account ASPNET.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="7cbf2-122">Ad esempio, in un computer denominato "WebDev", immettere webdev\ASPNET e fare clic su "OK".</span><span class="sxs-lookup"><span data-stu-id="7cbf2-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="7cbf2-123">Verificare che l'account ASPNET includa le caselle di controllo "Read &amp; Execute", "List Folder Contents" e "Read".</span><span class="sxs-lookup"><span data-stu-id="7cbf2-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="7cbf2-124">Fare clic su OK per chiudere la finestra di dialogo e salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="7cbf2-125">Se necessario, queste modifiche possono essere automatizzate tramite script o lo strumento "cacls. exe" fornito con Windows.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="7cbf2-126">Per ulteriori informazioni sull'account ASPNET, vedere il documento di [domande frequenti](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="7cbf2-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="7cbf2-127">Se una determinata applicazione Web si basa sulla presenza di autorizzazioni di scrittura o modifica per una cartella o un file specifico, è possibile concederlo seguendo la stessa procedura e selezionando le caselle di controllo "Write" e/o "Modify".</span><span class="sxs-lookup"><span data-stu-id="7cbf2-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="7cbf2-128">Nei computer che consentono a tutti o al gruppo di utenti di accedere in lettura a tali directory (ovvero la configurazione predefinita), non verrà rilevato alcun problema e i passaggi precedenti non saranno necessari.</span><span class="sxs-lookup"><span data-stu-id="7cbf2-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
