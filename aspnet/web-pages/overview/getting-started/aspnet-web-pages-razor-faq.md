---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) domande frequenti | Microsoft Docs
author: Rick-Anderson
description: Questo articolo elenca alcune domande frequenti su WebMatrix e ASP.NET Web Pages (Razor). Versioni del software utilizzate nell'esercitazione ASP.NET Web Pages (r....
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 27bfbe782455a5f8cf5096953c91712988b8dcab
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049098"
---
<a name="aspnet-web-pages-razor-faq"></a><span data-ttu-id="48688-104">Domande frequenti sulle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="48688-104">ASP.NET Web Pages (Razor) FAQ</span></span>
====================
<span data-ttu-id="48688-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="48688-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > <span data-ttu-id="48688-106">WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="48688-106">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="48688-107">Uso [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) oppure [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="48688-107">Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
>
> <span data-ttu-id="48688-108">Questo articolo elenca alcune domande frequenti su WebMatrix e ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="48688-108">This article lists some frequently asked questions about ASP.NET Web Pages (Razor) and WebMatrix.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="48688-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="48688-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="48688-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="48688-110">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="48688-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="48688-111">Visual Studio 2013</span></span>
> - <span data-ttu-id="48688-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="48688-112">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="48688-113">Questa esercitazione funziona anche con ASP.NET Web Pages 2, 2 WebMatrix e Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="48688-113">This tutorial also works with ASP.NET Web Pages 2, WebMatrix 2, and Visual Studio 2012.</span></span>


- [<span data-ttu-id="48688-114">Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="48688-114">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [<span data-ttu-id="48688-115">È necessario WebMatrix per funzionare con le pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-115">Do I need WebMatrix in order to work with Web Pages?</span></span>](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [<span data-ttu-id="48688-116">È possibile usare controlli di Web Form ASP.NET in una pagina di pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-116">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [<span data-ttu-id="48688-117">È possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="48688-117">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [<span data-ttu-id="48688-118">È necessario usare l'helper WebSecurity per supportare gli account di accesso?</span><span class="sxs-lookup"><span data-stu-id="48688-118">Do I have to use the WebSecurity helper to support logins?</span></span>](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [<span data-ttu-id="48688-119">ASP.NET Web Pages supporta HTML5?</span><span class="sxs-lookup"><span data-stu-id="48688-119">Does ASP.NET Web Pages support HTML5?</span></span>](#Does_ASP.NET_Web_Pages_support_HTML5)
- [<span data-ttu-id="48688-120">È possibile usare JavaScript e jQuery con pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-120">Can I use JavaScript and jQuery with Web Pages?</span></span>](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [<span data-ttu-id="48688-121">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="48688-121">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="48688-122">Per domande relative a errori e altri problemi, vedere la [Guida alla risoluzione dei problemi di ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).</span><span class="sxs-lookup"><span data-stu-id="48688-122">For questions about errors and other issues, see the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a><span data-ttu-id="48688-123">Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="48688-123">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>

<span data-ttu-id="48688-124">Tutte e tre sono tecnologie ASP.NET per la creazione di applicazioni web dinamiche:</span><span class="sxs-lookup"><span data-stu-id="48688-124">All three are ASP.NET technologies for creating dynamic web applications:</span></span>

- <span data-ttu-id="48688-125">ASP.NET Web Pages è incentrato sulla aggiunta dinamica del codice (lato server) e l'accesso al database per le pagine HTML e la sintassi semplice e leggera di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="48688-125">ASP.NET Web Pages focuses on adding dynamic (server-side) code and database access to HTML pages, and features simple and lightweight syntax.</span></span>
- <span data-ttu-id="48688-126">Web Form ASP.NET si basa su un modello a oggetti pagina e controlli tradizionale-tipo di finestra (pulsanti, elenchi e così via).</span><span class="sxs-lookup"><span data-stu-id="48688-126">ASP.NET Web Forms is based on a page object model and traditional window-type controls (buttons, lists, etc.).</span></span> <span data-ttu-id="48688-127">Web Form viene usato un modello basato su eventi familiare a coloro che hanno lavorato con sviluppo di (Windows Form) basata su client.</span><span class="sxs-lookup"><span data-stu-id="48688-127">Web Forms uses an event-based model that's familiar to those who've worked with client-based (Windows forms) development.</span></span>
- <span data-ttu-id="48688-128">ASP.NET MVC implementa il pattern model-view-controller per ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="48688-128">ASP.NET MVC implements the model-view-controller pattern for ASP.NET.</span></span> <span data-ttu-id="48688-129">L'attenzione viene focalizzata "separazione delle problematiche" (l'elaborazione dati e livelli dell'interfaccia utente).</span><span class="sxs-lookup"><span data-stu-id="48688-129">The emphasis is on "separation of concerns" (processing, data, and UI layers).</span></span>

<span data-ttu-id="48688-130">Tutti i tre Framework sono completamente supportati e continuano a essere sviluppati dal team ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="48688-130">All three frameworks are fully supported and continue to be developed by the ASP.NET team.</span></span> <span data-ttu-id="48688-131">In generale, la scelta di quali framework da usare dipende proprie competenze ed esperienze con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="48688-131">In general, the choice of which framework to use depends on your background and experience with ASP.NET.</span></span>

<span data-ttu-id="48688-132">In particolare, ASP.NET Web Pages è stato progettato per semplificare per gli utenti che già conoscono HTML per aggiungere l'elaborazione del server alle pagine.</span><span class="sxs-lookup"><span data-stu-id="48688-132">ASP.NET Web Pages in particular was designed to make it easy for people who already know HTML to add server processing to their pages.</span></span> <span data-ttu-id="48688-133">È una scelta ottimale per gli studenti, dilettanti, gli utenti in genere chi si accosta alla programmazione.</span><span class="sxs-lookup"><span data-stu-id="48688-133">It's a good choice for students, hobbyists, people in general who are new to programming.</span></span> <span data-ttu-id="48688-134">Può anche essere la scelta ideale per gli sviluppatori che hanno esperienza con le tecnologie web non ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="48688-134">It can also be a good choice for developers who have experience with non-ASP.NET web technologies.</span></span>

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a><span data-ttu-id="48688-135">È necessario WebMatrix per funzionare con le pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-135">Do I need WebMatrix in order to work with Web Pages?</span></span>

<span data-ttu-id="48688-136">No.</span><span class="sxs-lookup"><span data-stu-id="48688-136">No.</span></span> <span data-ttu-id="48688-137">WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="48688-137">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="48688-138">Uso [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) oppure [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="48688-138">Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>

<span data-ttu-id="48688-139">Se non si desidera usare Visual Studio o Visual Studio Code, è possibile installare i prodotti di componente utilizzando singolarmente [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="48688-139">If you don't want to use either Visual Studio or Visual Studio Code, you can install the component products individually using [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span> <span data-ttu-id="48688-140">Sono necessari i seguenti prodotti:</span><span class="sxs-lookup"><span data-stu-id="48688-140">You need the following products:</span></span>

- <span data-ttu-id="48688-141">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="48688-141">Microsoft .NET Framework 4.5</span></span>
- <span data-ttu-id="48688-142">ASP.NET MVC 5 (che viene installato anche il framework di ASP.NET Web Pages)</span><span class="sxs-lookup"><span data-stu-id="48688-142">ASP.NET MVC 5 (which installs the ASP.NET Web Pages framework as well)</span></span>
- <span data-ttu-id="48688-143">IIS Express (il server web)</span><span class="sxs-lookup"><span data-stu-id="48688-143">IIS Express (the web server)</span></span>
- <span data-ttu-id="48688-144">Microsoft SQL Server Compact 4.0 (database)</span><span class="sxs-lookup"><span data-stu-id="48688-144">Microsoft SQL Server Compact 4.0 (the database)</span></span>

<span data-ttu-id="48688-145">È possibile usare un editor di testo per modificare *. cshtml* (o *vbhtml*) pagine.</span><span class="sxs-lookup"><span data-stu-id="48688-145">You can use a text editor to edit *.cshtml* (or *.vbhtml*) pages.</span></span>

<span data-ttu-id="48688-146">La gestione di database SQL Server Compact (*sdf* file) senza uno strumento è un po' più difficoltosa.</span><span class="sxs-lookup"><span data-stu-id="48688-146">Managing SQL Server Compact databases (*.sdf* files) without a tool is a bit harder.</span></span> <span data-ttu-id="48688-147">Strumenti di Visual Studio containds per gestire *sdf* i database.</span><span class="sxs-lookup"><span data-stu-id="48688-147">Visual Studio containds tools for managing *.sdf* databases.</span></span> <span data-ttu-id="48688-148">È anche possibile eseguire i comandi SQL nel codice per eseguire molte attività di gestione di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="48688-148">You can also run SQL commands in code to perform many SQL Server management tasks.</span></span>

<span data-ttu-id="48688-149">Per testare *cshtml* pagine senza usare un ambiente di sviluppo integrato (IDE), è possibile distribuirli a un server attivo.</span><span class="sxs-lookup"><span data-stu-id="48688-149">To test *.cshtml* pages without using an integrated development environment (IDE), you can deploy them to a live server.</span></span> <span data-ttu-id="48688-150">(Vedere [è possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span><span class="sxs-lookup"><span data-stu-id="48688-150">(See [Can I deploy an ASP.NET Web Pages site without using WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span></span>

### <a name="running-iis-express-without-using-an-ide"></a><span data-ttu-id="48688-151">Esecuzione di IIS Express senza usare un IDE</span><span class="sxs-lookup"><span data-stu-id="48688-151">Running IIS Express without using an IDE</span></span>

<span data-ttu-id="48688-152">Se si installa IIS Express nel computer come server web, è possibile utilizzarlo per testare le pagine.</span><span class="sxs-lookup"><span data-stu-id="48688-152">If you install IIS Express on your computer as a web server, you can use that to test the pages.</span></span> <span data-ttu-id="48688-153">È possibile eseguire IIS Express dalla riga di comando e associarlo a un numero di porta specifico.</span><span class="sxs-lookup"><span data-stu-id="48688-153">You can run IIS Express from the command line and associate it with a specific port number.</span></span> <span data-ttu-id="48688-154">È quindi specificare tale porta quando si richiede *cshtml* file nel browser.</span><span class="sxs-lookup"><span data-stu-id="48688-154">You then specify that port when you request *.cshtml* files in your browser.</span></span>

<span data-ttu-id="48688-155">In Windows, aprire un prompt dei comandi con privilegi di amministratore e passare alla *c:\Programmi\Microsoft Files\IIS Express.*</span><span class="sxs-lookup"><span data-stu-id="48688-155">In Windows, open a command prompt with administrator privileges and change to *C:\Program Files\IIS Express.*</span></span> <span data-ttu-id="48688-156">(Per sistemi a 64 bit, usare la cartella *C:\Program Files (x86) \IIS Express.)* Immettere quindi il comando seguente, usando il percorso effettivo nel sito:</span><span class="sxs-lookup"><span data-stu-id="48688-156">(For 64-bit systems, use the folder *C:\Program Files (x86)\IIS Express.)* Then enter the following command, using the actual path to your site:</span></span>

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

<span data-ttu-id="48688-157">È possibile usare qualsiasi numero di porta che non è già riservato da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="48688-157">You can use any port number that isn't already reserved by some other process.</span></span> <span data-ttu-id="48688-158">(I numeri di porta superiore a 1024 sono in genere gratuiti). Per il `path` valore, utilizzare il percorso della cartella del sito Web in cui il *cshtml* file sono.</span><span class="sxs-lookup"><span data-stu-id="48688-158">(Port numbers above 1024 are typically free.) For the `path` value, use the path of the website folder where the *.cshtml* files are.</span></span>

<span data-ttu-id="48688-159">Dopo aver eseguito questo comando per configurare IIS Express per gestire le pagine, è possibile aprire un browser e passare a un *cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="48688-159">After you run this command to set up IIS Express to serve your pages, you can open a browser and browse to a *.cshtml* file.</span></span> <span data-ttu-id="48688-160">Usare un URL simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="48688-160">Use a URL like the following:</span></span>

`http://localhost:35896/default.cshtml`

<span data-ttu-id="48688-161">Per informazioni su IIS Express opzioni della riga di comando, immettere `iisexpress.exe /?` nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="48688-161">For help with IIS Express command line options, enter `iisexpress.exe /?` at the command line.</span></span>

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a><span data-ttu-id="48688-162">È possibile usare controlli di Web Form ASP.NET in una pagina di pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-162">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>

<span data-ttu-id="48688-163">No.</span><span class="sxs-lookup"><span data-stu-id="48688-163">No.</span></span> <span data-ttu-id="48688-164">Controlli Web Form, ad esempio la [casella di controllo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) (controllo), il [i controlli di convalida](https://msdn.microsoft.com/library/bwd43d0x)e il [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controllare funzionano solo nelle pagine Web Form (*aspx* file).</span><span class="sxs-lookup"><span data-stu-id="48688-164">Web Forms controls like the [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) control, the [validation controls](https://msdn.microsoft.com/library/bwd43d0x), and the [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) control only work in Web Forms pages (*.aspx* files).</span></span> <span data-ttu-id="48688-165">Questi controlli richiedono il framework della pagina Web Form.</span><span class="sxs-lookup"><span data-stu-id="48688-165">These controls require the Web Forms page framework.</span></span>

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a><span data-ttu-id="48688-166">È possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="48688-166">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>

<span data-ttu-id="48688-167">Sì.</span><span class="sxs-lookup"><span data-stu-id="48688-167">Yes.</span></span> <span data-ttu-id="48688-168">È possibile copiare manualmente i file di siti Web a un server (in genere usando FTP).</span><span class="sxs-lookup"><span data-stu-id="48688-168">You can manually copy website files to a server (typically by using FTP).</span></span> <span data-ttu-id="48688-169">Se si esegue una copia manuale, è inoltre necessario copiare i file che supportano SQL Server Compact (database).</span><span class="sxs-lookup"><span data-stu-id="48688-169">If you perform a manual copy, you also have to copy the files that support SQL Server Compact (the database).</span></span> <span data-ttu-id="48688-170">Per informazioni dettagliate, vedere il post di blog [applicazioni distribuzione Web Pages senza uno strumento](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).</span><span class="sxs-lookup"><span data-stu-id="48688-170">For details, see the blog entry [Deploying Web Pages applications without a tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).</span></span>

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a><span data-ttu-id="48688-171">È necessario usare l'helper WebSecurity per supportare gli account di accesso?</span><span class="sxs-lookup"><span data-stu-id="48688-171">Do I have to use the WebSecurity helper to support logins?</span></span>

<span data-ttu-id="48688-172">No.</span><span class="sxs-lookup"><span data-stu-id="48688-172">No.</span></span> <span data-ttu-id="48688-173">Il `SimpleMembership` provider che fa parte di ASP.NET Web Pages è una delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="48688-173">The `SimpleMembership` provider that is part of ASP.NET Web Pages is one option.</span></span> <span data-ttu-id="48688-174">Sono disponibili anche i provider di sicurezza che fanno parte di ASP.NET (che possono essere utilizzati per l'utilizzo di Web Form).</span><span class="sxs-lookup"><span data-stu-id="48688-174">The security providers that are part of ASP.NET (that you might be used to working with in Web Forms) are also available.</span></span> <span data-ttu-id="48688-175">Ad esempio, è possibile utilizzare autenticazione basata su form in ASP.NET Web Pages esattamente come farebbe in Web Form.</span><span class="sxs-lookup"><span data-stu-id="48688-175">For example, you can use forms authentication in ASP.NET Web Pages just as you would in Web Forms.</span></span> <span data-ttu-id="48688-176">Per un esempio di come usare i moduli di autenticazione, vedere l'articolo di supporto tecnico Microsoft [implementazione di autenticazione in un'applicazione ASP.NET usando C# .NET](https://support.microsoft.com/kb/301240).</span><span class="sxs-lookup"><span data-stu-id="48688-176">For one example of how to use forms authentication, see the Microsoft Support article [How To Implement Forms-Based Authentication in Your ASP.NET Application by Using C#.NET](https://support.microsoft.com/kb/301240).</span></span> <span data-ttu-id="48688-177">Per scaricare un esempio semplice, vedere [versione ASP.NET di "account di accesso &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).</span><span class="sxs-lookup"><span data-stu-id="48688-177">To download a simple example, see [ASP.NET version of "Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).</span></span>

<span data-ttu-id="48688-178">Per informazioni su come usare l'autenticazione di Windows, vedere il post di blog [autenticazione di Windows usando in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).</span><span class="sxs-lookup"><span data-stu-id="48688-178">For information about how to use Windows authentication, see the blog post [Using Windows authentication in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).</span></span>

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a><span data-ttu-id="48688-179">ASP.NET Web Pages supporta HTML5?</span><span class="sxs-lookup"><span data-stu-id="48688-179">Does ASP.NET Web Pages support HTML5?</span></span>

<span data-ttu-id="48688-180">Sì.</span><span class="sxs-lookup"><span data-stu-id="48688-180">Yes.</span></span> <span data-ttu-id="48688-181">Le pagine create con ASP.NET Web Pages (*. cshtml* oppure *vbhtml* pagine) sono essenzialmente le pagine HTML contenenti anche codice che viene eseguito nel server, prima che il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="48688-181">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are essentially HTML pages that also contain code that runs on the server, before the page is rendered.</span></span> <span data-ttu-id="48688-182">Purché il browser dell'utente supporta HTML5, è possibile usare HTML5 elementi in un *. cshtml* oppure *vbhtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="48688-182">As long as the user's browser supports HTML5, you can use HTML5 elements in a *.cshtml* or *.vbhtml* page.</span></span>

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a><span data-ttu-id="48688-183">È possibile usare JavaScript e jQuery con pagine Web?</span><span class="sxs-lookup"><span data-stu-id="48688-183">Can I use JavaScript and jQuery with Web Pages?</span></span>

<span data-ttu-id="48688-184">Certamente.</span><span class="sxs-lookup"><span data-stu-id="48688-184">Absolutely.</span></span> <span data-ttu-id="48688-185">Le pagine create con ASP.NET Web Pages (*. cshtml* oppure *vbhtml* pagine) sono solo pagine HTML con il codice server in essi contenuti.</span><span class="sxs-lookup"><span data-stu-id="48688-185">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are just HTML pages with server code in them.</span></span> <span data-ttu-id="48688-186">Pertanto, tutte le operazioni eseguibili in una normale pagina HTML dal usando JavaScript o jQuery è inoltre possibile eseguire un *. cshtml* oppure *vbhtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="48688-186">Therefore, anything you can do in a normal HTML page by using JavaScript or jQuery you can also do in a *.cshtml* or *.vbhtml* page.</span></span>

<span data-ttu-id="48688-187">Il **Starter Site** modello in WebMatrix contiene una serie di librerie jQuery.</span><span class="sxs-lookup"><span data-stu-id="48688-187">The **Starter Site** template in WebMatrix contains a number of jQuery libraries.</span></span> <span data-ttu-id="48688-188">Se si crea un sito con tale modello, il *gli script* cartella contiene una libreria di base di jQuery (*jquery 1.6.2.js)* e le librerie per la convalida di jQuery (*jquery.validate.js*e così via.).</span><span class="sxs-lookup"><span data-stu-id="48688-188">If you create a site by using that template, the *Scripts* folder contains a jQuery core library (*jquery-1.6.2.js)* and libraries for jQuery validation (*jquery.validate.js*, etc.).</span></span>

<span data-ttu-id="48688-189">Ecco alcuni post di blog che illustrano alcuni modi per usare jQuery con ASP.NET Web Pages:</span><span class="sxs-lookup"><span data-stu-id="48688-189">Here are some blog posts that illustrate ways to use jQuery with ASP.NET Web Pages:</span></span>

- <span data-ttu-id="48688-190">[Aggiunta di ASP.NET Web Pages usando WebMatrix jQuery per la bontà](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) da Rachel Appel</span><span class="sxs-lookup"><span data-stu-id="48688-190">[Adding jQuery Goodness to ASP.NET Web Pages using WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) by Rachel Appel</span></span>
- <span data-ttu-id="48688-191">[5 minuti: WebMatrix interfaccia utente di jQuery json + jQuery modelli](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) da Jonas Eriksson</span><span class="sxs-lookup"><span data-stu-id="48688-191">[5 min: WebMatrix + jQuery UI + json + jQuery templates](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) by Jonas Eriksson</span></span>
- <span data-ttu-id="48688-192">[WebMatrix e form jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) da Mike Brind</span><span class="sxs-lookup"><span data-stu-id="48688-192">[WebMatrix And jQuery Forms](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) by Mike Brind</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="48688-193">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="48688-193">Additional Resources</span></span>


[<span data-ttu-id="48688-194">Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="48688-194">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)

<span data-ttu-id="48688-195">[ASP.NET Web Pages e WebMatrix forum](https://forums.asp.net/1224.aspx/1?WebMatrix) sul sito Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="48688-195">[WebMatrix and ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix) on the ASP.NET website</span></span>