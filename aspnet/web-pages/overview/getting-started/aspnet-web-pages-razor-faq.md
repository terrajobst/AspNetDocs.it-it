---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Domande frequenti su Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo elenca alcune domande frequenti su Pagine Web ASP.NET (Razor) e WebMatrix. Versioni del software usate nell'esercitazione Pagine Web ASP.NET (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640931"
---
# <a name="aspnet-web-pages-razor-faq"></a>Domande frequenti sulle pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix non è più consigliato come Integrated Development Environment per Pagine Web ASP.NET. Usare [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o [Visual Studio Code](https://code.visualstudio.com/).
>
> Questo articolo elenca alcune domande frequenti su Pagine Web ASP.NET (Razor) e WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2, WebMatrix 2 e Visual Studio 2012.

- [Qual è la differenza tra Pagine Web ASP.NET, ASP.NET Web Forms e ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Per lavorare con le pagine Web è necessario WebMatrix?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [È possibile utilizzare I controlli Web Form ASP.NET in una pagina Web Pages?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [È possibile distribuire un sito Pagine Web ASP.NET senza usare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [È necessario usare l'helper WebSecurity per supportare gli accessi?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Pagine Web ASP.NET supporta HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [È possibile usare JavaScript e jQuery con le pagine Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Risorse aggiuntive](#AdditionalResources)

Per domande sugli errori e su altri problemi, vedere la [Guida alla risoluzione dei problemi di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Qual è la differenza tra Pagine Web ASP.NET, ASP.NET Web Forms e ASP.NET MVC?

Tutte e tre le tecnologie ASP.NET per la creazione di applicazioni Web dinamiche:

- Pagine Web ASP.NET è incentrato sull'aggiunta di codice dinamico (lato server) e di accesso al database alle pagine HTML e offre una sintassi semplice e leggera.
- I Web Form ASP.NET sono basati su un modello a oggetti di pagina e controlli di tipo finestra tradizionali (pulsanti, elenchi e così via). Web Forms usa un modello basato su eventi che è familiare a chi ha lavorato con lo sviluppo basato su client (Windows Form).
- ASP.NET MVC implementa il modello MVC (Model-View-Controller) per ASP.NET. L'enfasi è relativa alla separazione dei problemi (elaborazione, dati e livelli dell'interfaccia utente).

Tutti e tre i Framework sono completamente supportati e continuano a essere sviluppati dal team ASP.NET. In generale, la scelta del Framework da usare dipende dallo sfondo e dall'esperienza con ASP.NET.

Pagine Web ASP.NET in particolare è stato progettato in modo da semplificare per gli utenti che conoscono già il codice HTML aggiungere l'elaborazione del server alle pagine. Si tratta di una scelta ideale per studenti, hobbisti, persone in generale che non hanno familiarità con la programmazione. Può anche essere una scelta ideale per gli sviluppatori che hanno esperienza con le tecnologie Web non-ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Per lavorare con le pagine Web è necessario WebMatrix?

No. WebMatrix non è più consigliato come Integrated Development Environment per Pagine Web ASP.NET. Usare [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o [Visual Studio Code](https://code.visualstudio.com/).

Se non si desidera utilizzare Visual Studio o Visual Studio Code, è possibile installare i prodotti del componente singolarmente utilizzando [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Sono necessari i prodotti seguenti:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (che installa anche Pagine Web ASP.NET Framework)
- IIS Express (server Web)
- Microsoft SQL Server Compact 4,0 (database)

È possibile utilizzare un editor di testo per modificare le pagine con *estensione cshtml* (o *vbhtml*).

La gestione di database di SQL Server Compact (file*SDF* ) senza uno strumento è un po' più difficile. Visual Studio containds Tools per la gestione dei database *. sdf* . È anche possibile eseguire comandi SQL nel codice per eseguire numerose attività di gestione SQL Server.

Per testare le pagine con *estensione cshtml* senza usare un Integrated Development Environment (IDE), è possibile distribuirle in un server attivo. (Vedere è [possibile distribuire un sito pagine Web ASP.NET senza usare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Esecuzione di IIS Express senza utilizzare un IDE

Se si installa IIS Express nel computer come server Web, è possibile utilizzarlo per eseguire il test delle pagine. È possibile eseguire IIS Express dalla riga di comando e associarlo a un numero di porta specifico. La porta viene quindi specificata quando si richiedono i file con *estensione cshtml* nel browser.

In Windows aprire un prompt dei comandi con privilegi di amministratore e passare a *C:\Program \Programmi\iis Express.* Per i sistemi a 64 bit, utilizzare la cartella *c:\Programmi (x86) \IIS Express.* Quindi immettere il comando seguente, usando il percorso effettivo al sito:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

È possibile usare qualsiasi numero di porta che non sia già riservato da un altro processo. (I numeri di porta superiori a 1024 sono in genere gratuiti). Per il valore `path`, usare il percorso della cartella del sito Web in cui si trovano i file con *estensione cshtml* .

Dopo aver eseguito questo comando per configurare IIS Express per gestire le pagine, è possibile aprire un browser e passare a un file con *estensione cshtml* . Usare un URL simile al seguente:

`http://localhost:35896/default.cshtml`

Per informazioni sulle opzioni della riga di comando IIS Express, immettere `iisexpress.exe /?` dalla riga di comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>È possibile utilizzare I controlli Web Form ASP.NET in una pagina Web Pages?

No. I controlli Web Form come il controllo [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) , i [controlli di convalida](https://msdn.microsoft.com/library/bwd43d0x)e il controllo [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) funzionano solo nelle pagine Web Form (file con*estensione aspx* ). Questi controlli richiedono il Framework della pagina Web Form.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>È possibile distribuire un sito Pagine Web ASP.NET senza usare WebMatrix?

Sì. È possibile copiare manualmente i file del sito Web in un server (in genere tramite FTP). Se si esegue una copia manuale, è inoltre necessario copiare i file che supportano SQL Server Compact (il database). Per informazioni dettagliate, vedere il post di Blog relativo alla [distribuzione di applicazioni Web Pages senza uno strumento](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>È necessario usare l'helper WebSecurity per supportare gli accessi?

No. Il provider `SimpleMembership` che fa parte di Pagine Web ASP.NET è un'opzione. Sono disponibili anche i provider di sicurezza che fanno parte di ASP.NET (che potrebbero essere usati per lavorare in Web Forms). Ad esempio, è possibile usare l'autenticazione basata su form in Pagine Web ASP.NET come nei Web Form. Per un esempio di come usare l'autenticazione basata su form, vedere l'articolo supporto tecnico Microsoft [come implementare l'autenticazione basata su form nell'applicazione ASP.NET usando C#.NET](https://support.microsoft.com/kb/301240). Per scaricare un esempio semplice, vedere la [versione ASP.NET di "Login &amp; password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Per informazioni sull'uso dell'autenticazione di Windows, vedere il post di Blog relativo all' [uso dell'autenticazione di Windows in pagine Web ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Pagine Web ASP.NET supporta HTML5?

Sì. Le pagine create con Pagine Web ASP.NET (pagine con*estensione cshtml* o *vbhtml* ) sono essenzialmente pagine HTML che contengono anche codice in esecuzione nel server, prima che venga eseguito il rendering della pagina. Fino a quando il browser dell'utente supporta HTML5, è possibile usare gli elementi HTML5 in una pagina con *estensione cshtml* o *vbhtml* .

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>È possibile usare JavaScript e jQuery con le pagine Web?

Certamente. Le pagine create con Pagine Web ASP.NET (pagine con*estensione cshtml* o *vbhtml* ) sono solo pagine HTML con codice server. Pertanto, qualsiasi operazione che è possibile eseguire in una normale pagina HTML usando JavaScript o jQuery può essere eseguita anche in una pagina con *estensione cshtml* o *vbhtml* .

Il modello di **sito iniziale** in WebMatrix contiene una serie di librerie jQuery. Se si crea un sito usando tale modello, la cartella *script* contiene una libreria principale jQuery (*jQuery-1.6.2. js)* e le librerie per la convalida jQuery (*jQuery. Validate. js*e così via).

Di seguito sono riportati alcuni post di Blog in cui vengono illustrate le modalità di utilizzo di jQuery con Pagine Web ASP.NET:

- [Aggiunta di jQuery bontà a pagine Web ASP.NET tramite WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) di Rachel Appel
- [5 min: WebMatrix + interfaccia utente jQuery + json + modelli jQuery](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) di Jonas Eriksson
- [Moduli WebMatrix e jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) di Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Forum su WebMatrix e pagine Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) nel sito Web ASP.NET
