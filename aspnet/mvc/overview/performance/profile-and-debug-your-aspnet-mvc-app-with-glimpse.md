---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilare ed eseguire il debug dell'app ASP.NET MVC con una panoramica | Microsoft Docs
author: Rick-Anderson
description: Uno scorcio è una famiglia fiorente e in continua crescita di pacchetti NuGet open source che fornisce informazioni dettagliate sulle prestazioni, il debug e la diagnostica per ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457661"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Eseguire la profilatura e il debug dell'app ASP.NET MVC con Glimpse

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Uno scorcio è una famiglia fiorente e in continua crescita di pacchetti NuGet open source che offre informazioni dettagliate sulle prestazioni, il debug e la diagnostica per le app ASP.NET. Si tratta di un'operazione semplice da installare, leggera, estremamente veloce e da visualizzare le metriche delle prestazioni chiave nella parte inferiore di ogni pagina. Consente di eseguire il drill-down nell'app quando è necessario scoprire cosa accade nel server. Questo strumento offre informazioni molto utili che è consigliabile usare per tutto il ciclo di sviluppo, incluso l'ambiente di test di Azure. Sebbene [Fiddler](http://www.telerik.com/fiddler) e gli [strumenti di sviluppo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forniscano una visualizzazione lato client, offre una visualizzazione dettagliata del server. Questa esercitazione si concentra sull'uso dei pacchetti ASP.NET MVC e EF, ma sono disponibili molti altri pacchetti. Laddove possibile, mi collegherò alla [documentazione di anteprima](http://getglimpse.com/Docs/) appropriata che mi consentirò di gestire. Lo scorcio è un progetto open source, che può contribuire al codice sorgente e alla documentazione.

- [Installazione di un'occhiata](#ig)
- [Abilita lo scorcio per localhost](#eg)
- [Scheda sequenza temporale](#Time)
- [Associazione di modelli](#mb)
- [Route](#route)
- [Uso di una panoramica su Azure](#da)
- [Risorse aggiuntive](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installazione di un'occhiata

È possibile installare lo scorcio dalla console di gestione pacchetti NuGet o dalla console **Gestisci pacchetti NuGet** . Per questa demo, installerò i pacchetti Mvc5 e EF6:

![Panoramica dell'installazione di NuGet DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Cerca lo *scorcio. EF*

![Intravede. EF di NuGet install DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selezionando i **pacchetti installati**, è possibile visualizzare i moduli con lo sguardo dipendente installato:

![Installazione di pacchetti di anteprima da DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

I comandi seguenti consentono di installare i moduli MVC5 e EF6 dalla console di gestione pacchetti:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Abilita lo scorcio per localhost

Passare a http://localhost:&lt;p Ort #&gt;/Glimpse.axd e fare clic sul pulsante di <strong>attivazione dello scorcio</strong> .

![Pagina di anteprima del AXD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se è visualizzata la barra Preferiti, è possibile trascinare e rilasciare i pulsanti degli scorci e aggiungerli come bookmarklets:

![Internet Explorer con i bookmarklet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

È ora possibile spostarsi all'interno dell'app e il riquadro delle **intestazioni** (HUD) viene visualizzato nella parte inferiore della pagina.

![Pagina Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

La [pagina dello scorcio HUD](http://getglimpse.com/Docs/Heads-up-Display) descrive in dettaglio le informazioni di temporizzazione visualizzate sopra. I dati sulle prestazioni non intrusivi visualizzati dall'HUD possono notificare immediatamente un problema, prima di arrivare al ciclo di test. Facendo clic sul &quot;g&quot; nell'angolo in basso a destra viene visualizzata la finestra di anteprima:

![Pannello degli scorci](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Nell'immagine precedente è selezionata la [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) , che mostra i dettagli dell'intervallo delle azioni e dei filtri nella pipeline. È possibile visualizzare il [timer di filtro di controllo Stop Watch](http://www.nuget.org/packages/StopWatch/) alla fase 6 della pipeline. Sebbene il timer di peso leggero possa fornire dati di profilo/temporizzazione utili, manca tutto il tempo impiegato per l'autorizzazione e il rendering della visualizzazione. Per informazioni sul timer [, vedere profilo e ora dell'app ASP.NET MVC fino a Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Nella pagina [schede](http://getglimpse.com/Docs/Tabs) sono disponibili collegamenti a informazioni dettagliate su ogni scheda.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Scheda sequenza temporale

È stata modificata l' [esercitazione EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) in attesa di Tom Dykstra con la modifica del codice seguente per il controller degli insegnanti:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Il codice precedente consente di passare la stringa di query (`eager`) per controllare il caricamento di dati eager o esplicito. Nell'immagine seguente viene usato il caricamento esplicito e la pagina di temporizzazione Mostra ogni registrazione caricata nel metodo di azione `Index`:

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Nel codice seguente viene specificato eager e ogni registrazione viene recuperata dopo la chiamata di `Index` vista:

![è stato specificato eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

È possibile passare il puntatore del mouse su un segmento temporale per ottenere informazioni dettagliate sugli intervalli:

![passare il mouse per visualizzare la tempistica dettagliata](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Associazione di modelli

La [scheda associazione modello](http://getglimpse.com/Docs/Model-Binding-Tab) fornisce un'ampia gamma di informazioni che consentono di comprendere in che modo le variabili del modulo sono associate e perché alcune non sono associate come previsto. L'immagine seguente mostra il **?** , su cui è possibile fare clic per visualizzare la pagina della guida dello scorcio relativa a tale funzionalità.

![visualizzazione associazione modello](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Route

 Questa scheda consente di eseguire il debug e comprendere il routing. Nell'immagine seguente è selezionata la route del prodotto (e viene visualizzata in verde, una convenzione di uno scorcio). vengono visualizzati anche ![nome prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vincoli di route, aree e token di dati. Per ulteriori informazioni, vedere la pagina relativa agli [scorci delle route](http://getglimpse.com/Docs/Routes-Tab) e [al routing degli attributi in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) . 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Uso di una panoramica su Azure

Con i criteri di sicurezza predefiniti è possibile visualizzare solo i dati degli scorci dall'host locale. È possibile modificare questo criterio di sicurezza in modo che sia possibile visualizzare i dati in un server remoto, ad esempio un'app Web in Azure. Per gli ambienti di test in Azure, aggiungere il segno evidenziato fino alla fine del file *Web. config* per abilitare gli scorci:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con questa modifica, qualsiasi utente può visualizzare i dati degli sguardi in un sito remoto. Si consiglia di aggiungere il markup precedente a un profilo di pubblicazione in modo che venga distribuito un solo quando si usa il profilo di pubblicazione (ad esempio, il profilo di test di Azure). Per limitare i dati, si aggiungerà il ruolo `canViewGlimpseData` e si consentirà solo agli utenti in questo ruolo di visualizzare i dati di scorcio.

Rimuovere i commenti dal file *GlimpseSecurityPolicy.cs* e modificare la chiamata [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) da `Administrator` al ruolo `canViewGlimpseData`:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sicurezza: i dati avanzati offerti da un'occhiata possono esporre la sicurezza dell'app. Microsoft non ha eseguito un controllo di sicurezza per l'uso nelle app di produzione.

Per informazioni sull'aggiunta di ruoli, vedere l'esercitazione [distribuire un'app web ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Configurazione di anteprima](http://getglimpse.com/Docs/Configuration) -pagina documento sulla configurazione di schede, criteri di runtime, registrazione e altro ancora.
