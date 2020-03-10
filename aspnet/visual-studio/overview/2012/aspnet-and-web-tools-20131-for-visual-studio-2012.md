---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione di ASP.NET and Web Tools 2013,1 per Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Questo documento descrive la versione di ASP.NET and Web Tools 2013,1 per Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578442"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Note sulla versione di ASP.NET and Web Tools 2013.1 per Visual Studio 2012

[Microsoft](https://github.com/microsoft)

> Questo documento descrive la versione di ASP.NET and Web Tools 2013,1 per Visual Studio 2012.

## <a name="contents"></a>Contenuto

- [Note sull'installazione](#install)
- [Requisiti software](#requirements)
- Nuove funzionalità di ASP.NET and Web Tools 2013,1 per Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modelli](#templates)

        - [Modello ASP.NET MVC 5](#mvc5template)
        - [Modello di API Web ASP.NET 2](#apitemplate)
        - [Modelli di elemento](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Impalcatura ASP.NET](#scaffold)
    - [Editor Razor](#razor)
    - [NuGet 2.7](#nuget)
- Problemi noti e modifiche di rilievo

    - [Impalcatura ASP.NET](#issuescaffolding)

        - [Impalcature MVC e Web API-HTTP 404, errore non trovato](#404issue)
        - [Visual Studio Express 2012 per il Web smette di funzionare dopo l'aggiunta di un elemento con impalcatura](#expressissue)
    - [Razor 3 ASP.NET](#issuerazor)

        - [La visualizzazione del file cshtml con o F5 causa un errore del server](#browseissue)
        - [Riscrittura URL e tilde (~)](#rewriteissue)
    - [Modelli](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Note di installazione

[Installa](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013,1 per Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisiti software

È necessario che sia installato Visual Studio 2012 o Visual Studio Express 2012 per il Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuove funzionalità di ASP.NET and Web Tools 2013,1 per Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Quando si esegue il patibolo di controller e visualizzazioni MVC 5, il markup per le visualizzazioni USA [bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modelli

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modello ASP.NET MVC 5

È stato aggiunto un nuovo modello MVC 5. Fa riferimento ai pacchetti NuGet di MVC 5 più recenti ed è possibile usare l'impalcatura per aggiungere controller e visualizzazioni.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modello di API Web ASP.NET 2

È stato aggiunto un nuovo modello API Web 2. Fa riferimento ai pacchetti NuGet dell'API Web 2 più recenti ed è possibile usare l'impalcatura per aggiungere controller e visualizzazioni.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modelli di elementi

Sono stati aggiunti nuovi modelli di elemento per le visualizzazioni MVC 5, le pagine Web (Razor 3) e i controller API Web 2. Installano i pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Quando si esegue l'impalcatura di un controller MVC o API Web usando Entity Framework, viene usato il Framework 6. Per ulteriori informazioni su Entity Framework, vedere la [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).

È anche possibile scaricare e installare gli strumenti di Entity Framework 6 per Visual Studio 2012. Vedere il [Entity Framework Get](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Impalcatura ASP.NET

L'impalcatura ASP.NET è un Framework per la generazione di codice per le applicazioni Web ASP.NET. Consente di aggiungere in modo semplice codice standard al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, l'impalcatura era limitata ai progetti MVC ASP.NET. Con questo aggiornamento, è ora possibile usare l'impalcatura per qualsiasi progetto ASP.NET, inclusi i Web Form. Questo aggiornamento non supporta la generazione di pagine per un progetto Web Form, ma è comunque possibile usare l'impalcatura con i Web form aggiungendo dipendenze MVC al progetto. Il supporto per la generazione di pagine per Web Form verrà aggiunto in un aggiornamento futuro.

Quando si usa l'impalcatura, si garantisce che tutte le dipendenze obbligatorie siano installate nel progetto. Se ad esempio si inizia con un progetto Web Form ASP.NET e quindi si usa l'impalcatura per aggiungere un controller API Web, i pacchetti NuGet e i riferimenti necessari vengono aggiunti automaticamente al progetto.

Per aggiungere l'impalcatura MVC a un progetto Web Form, aggiungere un **nuovo elemento con impalcatura** e selezionare le **dipendenze MVC 5** nella finestra di dialogo. Sono disponibili due opzioni per l'impalcatura MVC; Minimo e completo. Se si seleziona minimal, al progetto vengono aggiunti solo i pacchetti NuGet e i riferimenti per ASP.NET MVC. Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC.

Il supporto per i controller asincroni di ponteggi usa le nuove funzionalità asincrone di Entity Framework 6.

Per altre informazioni ed esercitazioni, vedere [Cenni preliminari sull'impalcatura ASP.NET](../2013/aspnet-scaffolding-overview.md). Queste esercitazioni mostrano le impalcature con Visual Studio 2013, ma sono valide anche per ASP.NET and Web Tools 2013,1 per Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Editor Razor

Con questo aggiornamento, Visual Studio 2012 supporta ora gli strumenti e le modifiche di Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 include un set completo di nuove funzionalità descritte in dettaglio nelle note sulla [versione di nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet Elimina la necessità per gli utenti di consentire in modo esplicito a NuGet di ripristinare i pacchetti mancanti. Quando si installa NuGet 2,7, gli utenti acconsentono in modo implicito a ripristinare automaticamente i pacchetti mancanti. Gli utenti possono rifiutare esplicitamente il ripristino del pacchetto tramite le impostazioni di NuGet in Visual Studio. Questa modifica semplifica il funzionamento del ripristino del pacchetto.

## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Impalcatura ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Impalcature MVC e Web API-HTTP 404, errore non trovato

Se si verifica un errore durante l'aggiunta di un elemento con impalcatura a un progetto, è possibile che il progetto venga lasciato in uno stato incoerente. Verrà eseguito il rollback di alcune delle modifiche apportate all'impalcatura, ma non verrà eseguito il rollback di altre modifiche, ad esempio i pacchetti NuGet installati. Se viene eseguito il rollback delle modifiche della configurazione di routing, gli utenti riceveranno un errore HTTP 404 quando si passa a elementi con impalcature.

Per correggere l'errore per MVC, aggiungere un nuovo elemento con impalcatura e selezionare le dipendenze MVC 5 (minime o complete). Questo processo consente di aggiungere tutte le modifiche necessarie al progetto.

Per correggere l'errore per l'API Web:

1. Aggiungere la classe WebApiConfig seguente al progetto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurare WebApiConfig. Register nell'applicazione\_metodo Start in Global. asax come indicato di seguito:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 per il Web smette di funzionare dopo l'aggiunta di un elemento con impalcatura

Se Visual Studio Express 2012 per il Web smette di funzionare dopo l'aggiunta di un elemento con impalcatura con Entity Framework, ad esempio un controller Web API 2 con azioni, usando Entity Framework, è possibile che Visual Studio Express non è riuscito a caricare l'immagine nativa di un assembly dipendente da System. Web. Extensions.

Per risolvere il problema, configurare Visual Studio Express per utilizzare l'immagine MSIL di System. Web. Extensions:

1. Aprire il prompt dei comandi in modalità amministratore.
2. Passare a%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE o% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (per Windows a 64 bit).
3. Aprire VWDExpress. exe. config in un editor di testo.
4. Aggiungere la riga seguente sotto l'elemento &lt;Configuration&gt;/&lt;Runtime&gt;:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Riavviare Visual Studio Express 2012 per il Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>Razor 3 ASP.NET

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>La visualizzazione del file cshtml con o F5 causa un errore del server

Quando si crea un progetto MVC 5 in Visual Studio 2012 (oppure si apre in Visual Studio 2012 un progetto MVC 5 creato in Visual Studio 2013) e si tenta di visualizzare un file cshtml tramite Sfoglia con o F5, si riceverà un errore che indica che l'errore è stato- **server nell'applicazione '/'** . Il server tenta di passare a `http://localhost:XXXX/Views/../XXXX.cshtml`

Per risolvere questo problema, modificare l'impostazione **azione di avvio** nel progetto in una **pagina specifica**. Non è necessario specificare un valore per la pagina.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Dopo avere apportato questa modifica, la selezione di F5 passa alla radice dell'applicazione (`http://localhost:XXXX`). Questo comportamento non corrisponde al comportamento per i progetti MVC 5 in Visual Studio 2013, in cui l'impostazione della **pagina corrente** avvia la pagina Apri.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Riscrittura URL e tilde (~)

Dopo l'aggiornamento a ASP.NET Razor 3 o ASP.NET MVC 5, la notazione tilde (~) potrebbe non funzionare più correttamente se si usano le riscritture URL. La riscrittura URL influiscono sulla notazione tilde (~) negli elementi HTML, ad esempio &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;collegamento/&gt;e, di conseguenza, non viene più eseguito il mapping della tilde alla directory radice.

Ad esempio, se si riscrivono le richieste per **ASP.net/content** in **ASP.NET**, l'attributo href in &lt;a href = "~/Content/"/&gt; viene risolto in **/content/content/** invece che **/** . Per evitare questa modifica, è possibile impostare il contesto di **IIS\_WasUrlRewritten** su false in ogni pagina Web o nell' **applicazione\_BeginRequest** in Global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modelli

Quando si creano progetti MVC ASP.NET con Visual Studio 2012 in Windows 8.1 o Windows Server 2012 R2, Visual Studio Visualizza un messaggio di errore che indica che la configurazione del Web [URL] per ASP.NET 4,5 non è riuscita. "

![errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Questo errore viene visualizzato perché Visual Studio 2012 non Abilita la funzionalità ASP.NET 4,5 quando viene installata nelle versioni di Windows. Per abilitare ASP.NET 4,5, eseguire la procedura descritta in [attivare o disattivare le funzionalità Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

In alternativa, è possibile abilitare ASP.NET 4,5 tramite la riga di comando.

1. Aprire il prompt dei comandi in modalità amministratore.
2. Eseguire il comando seguente per abilitare ASP.NET 4,5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
