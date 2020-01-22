---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creazione di progetti Web ASP.NET in Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: In questo argomento vengono illustrate le opzioni per la creazione di progetti Web ASP.NET in Visual Studio 2013 con Update 3, Ecco alcune delle nuove funzionalità per lo sviluppo Web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519271"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Creazione di progetti Web ASP.NET in Visual Studio 2013

di [Tom Dykstra](https://github.com/tdykstra)

> In questo argomento vengono illustrate le opzioni per la creazione di progetti Web ASP.NET in Visual Studio 2013 con Update 3
> 
> Di seguito sono riportate alcune delle nuove funzionalità per lo sviluppo Web rispetto alle versioni precedenti di Visual Studio:
> 
> - Interfaccia utente semplice per la creazione di progetti che offrono [supporto per più framework ASP.NET](#add) (Web Form, MVC e API Web).
> - [ASP.NET Identity](#indauth), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software di hosting Web diverso da IIS.
> - L'uso di [bootstrap](#bootstrap) per fornire funzionalità di progettazione e di tema reattive.
> - Nuove funzionalità per i Web Form che erano disponibili solo per MVC, ad esempio la [creazione automatica](#testproj) di un progetto di test e un [modello di sito Intranet](#winauth).
> 
> Per informazioni su come creare progetti Web per servizi cloud di Azure o servizi mobili di Azure, vedere [Introduzione a servizi cloud di Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) e [creazione di un'app leaderboard con il back-end .NET di servizi mobili](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)di Azure.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

Questo articolo si applica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [aggiornamento 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installato.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Progetti di applicazioni Web rispetto ai progetti di siti Web

ASP.NET offre la possibilità di scegliere tra due tipi di progetti web: *progetti di applicazione Web* e *progetti di siti Web*. Si consiglia di usare i progetti di applicazioni Web per il nuovo sviluppo e questo articolo si applica solo ai progetti di applicazione Web. Per ulteriori informazioni, vedere [progetti di applicazioni Web rispetto ai progetti di siti Web in Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) nel sito MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Panoramica della creazione di un progetto di applicazione Web

Nei passaggi seguenti viene illustrato come creare un progetto Web:

1. Fare clic su **nuovo progetto** nella pagina **iniziale** o nel menu **file** .
2. Nella finestra di dialogo **nuovo progetto** fare clic su **Web** nel riquadro sinistro e **ASP.NET applicazione Web** nel riquadro centrale.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image1.png)

    È possibile scegliere **cloud** nel riquadro sinistro per creare un [servizio cloud di Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), un [servizio mobile](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)di Azure o [Azure processo Web](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). In questo argomento non vengono illustrati tali modelli.
3. Nel riquadro destro fare clic sulla casella **di controllo aggiungi Application Insights al progetto** se si desidera il monitoraggio dello stato e dell'utilizzo dell'applicazione. Per altre informazioni, vedere [Monitorare le prestazioni di applicazioni Web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Specificare il **nome**del progetto, la **posizione**e altre opzioni, quindi fare clic su **OK**.

    Verrà visualizzata la finestra di dialogo **nuovo progetto ASP.NET** .

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Fare clic su un modello.

    ![Seleziona un modello](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se si desidera aggiungere il supporto per Framework aggiuntivi non inclusi nel modello, fare clic sulla casella di controllo appropriata. Nell'esempio illustrato, è possibile aggiungere MVC e/o l'API Web a un progetto Web Form.

    ![Aggiungi Framework](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se si desidera aggiungere un progetto di unit test, fare clic su **Aggiungi unit test**.

    ![Aggiungi test unità](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se si desidera un metodo di autenticazione diverso da quello fornito dal modello per impostazione predefinita, fare clic su **Modifica autenticazione**.

    ![Pulsante Configura autenticazione](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Finestra di dialogo Configura autenticazione](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Creare un'app Web o una macchina virtuale in Azure

Visual Studio include funzionalità che facilitano l'uso dei servizi di Azure per l'hosting di applicazioni Web. Ad esempio, è possibile eseguire tutte le operazioni seguenti direttamente dall'IDE di Visual Studio:

- Crea e Gestisci app Web o macchine virtuali che rendono l'applicazione disponibile su Internet.
- Consente di visualizzare i log creati dall'applicazione durante l'esecuzione nel cloud.
- Esecuzione in modalità debug in remoto mentre l'applicazione viene eseguita nel cloud.
- Visualizzare e gestire altri servizi di Azure, come i database SQL.

È possibile [creare un account di Azure](https://www.windowsazure.com/pricing/free-trial/) che include servizi di base, ad esempio app Web gratuite e, per gli abbonati MSDN, è possibile [attivare i vantaggi](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) che forniscono crediti mensili per altri servizi di Azure. 

Per impostazione predefinita, la finestra di dialogo **nuovo progetto ASP.NET** consente di creare un'app Web o una macchina virtuale per un nuovo progetto Web. Se non si vuole creare una nuova app Web o macchina virtuale, deselezionare la casella **di controllo host nel cloud** .

![Creare risorse remote](creating-web-projects-in-visual-studio/_static/image8.png)

La didascalia della casella di controllo potrebbe essere **ospitata nel cloud** o **creare risorse remote**e in entrambi i casi l'effetto è lo stesso. Se si lascia selezionata la casella di controllo, Visual Studio crea un'app Web nel servizio app Azure per impostazione predefinita. Se lo si preferisce, è possibile usare la casella di riepilogo a discesa per modificarla in una **macchina virtuale** . Se non è già stato effettuato l'accesso ad Azure, vengono richieste le credenziali di Azure. Una volta eseguito l'accesso, viene visualizzata una finestra di dialogo che consente di configurare le risorse che Visual Studio creerà per il progetto. La figura seguente mostra la finestra di dialogo per un'app Web; Se si sceglie di creare una macchina virtuale, vengono visualizzate opzioni diverse.

![Configurare le impostazioni di app Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Per altre informazioni su come usare questo processo per la creazione di risorse di Azure, vedere [Introduzione ad Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [creazione di una macchina virtuale per un sito Web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Nella parte restante di questo articolo vengono fornite ulteriori informazioni sui modelli disponibili e sulle relative opzioni. L'articolo introduce anche bootstrap, il layout e il Framework di tema utilizzati nei modelli.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelli di progetto Web Visual Studio 2013

Visual Studio USA i modelli per la creazione di progetti Web. Un modello di progetto può creare file e cartelle nel nuovo progetto, installare pacchetti NuGet e fornire codice di esempio per un'applicazione di lavoro rudimentale. I modelli implementano gli standard Web più recenti e hanno lo scopo di illustrare le procedure consigliate per l'uso delle tecnologie ASP.NET, oltre a offrire una rapida introduzione alla creazione di un'applicazione personalizzata.

Visual Studio 2013 offre le opzioni seguenti per i modelli di progetto Web per i progetti destinati a .NET 4,5 o versioni successive di .NET Framework:

- [Modello vuoto](#empty)
- [Modello di Web Form](#wf)
- [Modello MVC](#mvc)
- [Modello API Web](#webapi)
- [Modello di applicazione a pagina singola](#spa)
- [Modello di servizio mobile di Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelli di Visual Studio 2012](#vs2012)

È anche possibile installare un'estensione di Visual Studio che fornisce un [modello di Facebook](#facebook).

Per informazioni su come creare progetti destinati a .NET 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo argomento.

Per informazioni su come creare applicazioni ASP.NET per client mobili, vedere [supporto per dispositivi mobili in ASP.NET](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modello vuoto

Il modello vuoto fornisce le cartelle e i file minimi per un'app Web ASP.NET, ad esempio un file di progetto (con*estensione csproj* o. *vbproj*) e un file *Web. config* . È possibile aggiungere il supporto per Web Form, MVC e/o API Web usando le caselle di controllo nell'etichetta **Aggiungi cartelle e riferimenti principali per:** .

Per il modello vuoto non sono disponibili opzioni di autenticazione. La funzionalità di autenticazione è implementata nelle applicazioni di esempio e il modello vuoto non crea un'applicazione di esempio.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modello di Web Form

Il framework Web Form fornisce le funzionalità seguenti che consentono di creare rapidamente siti Web avanzati nelle funzionalità di interfaccia utente e accesso ai dati:

- Una finestra di progettazione WYSIWYG in Visual Studio.
- Controlli server che eseguono il rendering del codice HTML e che è possibile personalizzare impostando proprietà e stili.
- Ampia gamma di controlli per l'accesso ai dati e la visualizzazione dei dati.
- Modello di eventi che espone gli eventi che è possibile programmare come si programma un'applicazione client, ad esempio WPF.
- Conservazione automatica dello stato (dati) tra le richieste HTTP.

In generale, la creazione di un'applicazione Web Form richiede un minor sforzo di programmazione rispetto alla creazione della stessa applicazione tramite il framework ASP.NET MVC. Tuttavia, Web Form non è solo per lo sviluppo rapido di applicazioni. Molti Framework e applicazioni commerciali complesse sono basati su Web Form.

Poiché una pagina Web Form e i controlli della pagina generano automaticamente gran parte del markup inviato al browser, non è disponibile il tipo di controllo con granularità fine sul codice HTML offerto da ASP.NET MVC. Il modello dichiarativo per la configurazione di pagine e controlli riduce al minimo la quantità di codice da scrivere, ma nasconde parte del comportamento di HTML e HTTP. Ad esempio, non è sempre possibile specificare esattamente il markup che può essere generato da un controllo.

Il framework Web Forms non si presta con facilità come ASP.NET MVC alle procedure di sviluppo basate su modelli come [lo sviluppo](http://en.wikipedia.org/wiki/Test-driven_development)basato su test, la [separazione dei problemi](http://en.wikipedia.org/wiki/Separation_of_concerns), l' [inversione del controllo](http://en.wikipedia.org/wiki/Inversion_of_control)e l' [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Se si desidera scrivere codice in questo modo, è possibile; non è così automatico come nel framework ASP.NET MVC. Il progetto [MVP per Web form ASP.NET](http://webformsmvp.com/) illustra un approccio che semplifica la separazione dei problemi e la testabilità, mantenendo al tempo stesso lo sviluppo rapido per il quale i moduli Web sono stati creati per la distribuzione. Microsoft SharePoint si basa su Web Form MVP.

Il modello Web Forms consente di creare un'applicazione Web Form di esempio che usa [bootstrap](#bootstrap) per fornire funzionalità di progettazione e di tema reattive. Nella figura seguente viene illustrato il home page.

![App modello di Web form home page](creating-web-projects-in-visual-studio/_static/image10.png)

Per ulteriori informazioni su Web Form, vedere [ASP.NET Web Forms](https://asp.net/web-forms). Per informazioni sulle funzionalità del modello Web Forms, vedere compilazione di [un'applicazione Web Forms di base con Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modello MVC

ASP.NET MVC è stato progettato per semplificare le procedure di sviluppo basate su modelli come [lo sviluppo](http://en.wikipedia.org/wiki/Test-driven_development)basato su test, la [separazione dei problemi](http://en.wikipedia.org/wiki/Separation_of_concerns), l' [inversione del controllo](http://en.wikipedia.org/wiki/Inversion_of_control)e l' [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Il Framework incoraggia la separazione del livello di logica di business di un'applicazione Web dal livello di presentazione. Suddividendo l'applicazione in modelli (M), viste (V) e controller (C), ASP.NET MVC può semplificare la gestione della complessità nelle applicazioni di dimensioni maggiori.

Con ASP.NET MVC, è possibile lavorare più direttamente con HTML e HTTP anziché con i Web Form. Ad esempio, i Web Form possono mantenere automaticamente lo stato tra le richieste HTTP, ma è necessario scrivere codice in modo esplicito in MVC. Il vantaggio del modello MVC è che consente di assumere il controllo completo sulle operazioni svolte dall'applicazione e sul suo comportamento nell'ambiente Web. Lo svantaggio è che è necessario scrivere altro codice.

MVC è stato progettato per essere estendibile e offre agli sviluppatori la possibilità di personalizzare il Framework in base alle esigenze dell'applicazione. Il codice sorgente di ASP.NET MVC è inoltre disponibile con una licenza OSI.

Il modello MVC crea un'applicazione MVC 5 di esempio che usa [bootstrap](#bootstrap) per fornire funzionalità di progettazione e di tema reattive. Nella figura seguente viene illustrato il home page.

![Applicazione di esempio MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Per ulteriori informazioni su MVC, vedere [ASP.NET MVC](https://asp.net/mvc). Per informazioni su come selezionare il modello MVC 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo articolo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modello API Web

Il modello API Web crea un servizio Web di esempio basato su API Web, incluse le pagine della Guida dell'API basate su MVC.

ASP.NET Web API è un framework che consente di creare facilmente servizi HTTP in grado di raggiungere un ampio numero di client, inclusi browser e dispositivi mobili. API Web ASP.NET è una piattaforma ideale per la creazione di servizi RESTful nel .NET Framework.

Il modello API Web crea un servizio Web di esempio. Nelle illustrazioni seguenti vengono mostrate le pagine della Guida di esempio.

![Pagina della Guida dell'API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Pagina della Guida dell'API Web per GET API](creating-web-projects-in-visual-studio/_static/image13.png)

Per ulteriori informazioni sull'API Web, vedere [API Web ASP.NET](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Single Page Application Template (in inglese)

Il modello applicazione a pagina singola (SPA) crea un'applicazione di esempio che usa JavaScript, HTML 5 e [Knockout](http://knockoutjs.com/) nel client e API Web ASP.NET sul server.

L'unica opzione di autenticazione per il modello SPA è [singoli account utente](#indauth).

Nella figura seguente viene illustrato lo stato iniziale dell'applicazione di esempio compilata dal modello di SPA.

![Applicazione di esempio SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Per informazioni su come creare un'applicazione usando il modello di applicazione a singola pagina, vedere [API Web-servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md).

Per altre informazioni sulle applicazioni a pagina singola ASP.NET e sui modelli di SPA aggiuntivi che usano framework JavaScript diversi da knockout, vedere le risorse seguenti:

- [Applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).
- [Informazioni sulle funzionalità di sicurezza nel modello di SPA per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applicazioni a singola pagina: creare app Web moderne e reattive con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modello di Facebook

È possibile installare un' [estensione di Visual Studio che fornisce un modello di Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Questo modello consente di creare un'applicazione di esempio progettata per l'esecuzione all'interno del sito Web Facebook. Si basa su ASP.NET MVC e usa l'API Web per la funzionalità di aggiornamento in tempo reale.

Non sono disponibili opzioni di autenticazione per il modello Facebook perché le applicazioni Facebook vengono eseguite nel sito Facebook e si basano sull'autenticazione di Facebook.

Per altre informazioni sulle applicazioni ASP.NET per Facebook, vedere [aggiornamento dell'API di Facebook per MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelli di Visual Studio 2012

La finestra di dialogo di creazione del progetto Web Visual Studio 2013 non fornisce l'accesso ad alcuni modelli disponibili in Visual Studio 2012. Se si vuole usare uno di questi modelli, è possibile fare clic sul nodo Visual Studio 2012 nel riquadro sinistro della finestra di dialogo nuovo progetto di Visual Studio.

![Modelli di Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Il nodo **Visual Studio 2012** consente di scegliere i modelli Web seguenti a cui non si è autorizzati ad accedere nell'elenco predefinito di modelli per Visual Studio 2013:

- Applicazione Web ASP.NET MVC 4
- Applicazione Web entità ASP.NET Dynamic Data
- Controllo server AJAX ASP.NET
- Estensione controllo server AJAX ASP.NET
- Controllo server ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap nei modelli di progetto Web Visual Studio 2013

I modelli di progetto Visual Studio 2013 usano [bootstrap](http://getbootstrap.com/), un layout e un Framework di tema creati da Twitter. Bootstrap USA CSS3 per fornire una progettazione reattiva, il che significa che i layout possono adattarsi dinamicamente alle diverse dimensioni della finestra del browser. Ad esempio, in una finestra del browser ampia il home page creato dal modello Web Forms è simile a quanto illustrato nella figura seguente:

![App modello di Web form home page](creating-web-projects-in-visual-studio/_static/image16.png)

Rendere la finestra più stretta e le colonne disposte orizzontalmente si spostano in disposizione verticale:

![Disposizione colonne verticali bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Restringere ulteriormente la finestra e il menu in alto orizzontale diventa un'icona su cui è possibile fare clic per espandersi in un menu orientato verticalmente:

![Icona del menu bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu verticale bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

È anche possibile usare la funzionalità di tema di bootstrap per applicare facilmente una modifica nell'aspetto dell'applicazione. È ad esempio possibile eseguire i passaggi seguenti per modificare il tema.

1. Nel browser passare a [http://Bootswatch.com](http://Bootswatch.com), scegliere un tema e quindi fare clic su **download**. (Scarica *bootstrap. min. CSS* per impostazione predefinita; se si vuole esaminare il codice CSS, ottenere *bootstrap. CSS* invece della versione minimizzati).
2. Copiare il contenuto del file CSS scaricato.
3. In Visual Studio creare un nuovo file di **foglio di stile** denominato *bootstrap-Theme. CSS* nella cartella *Content* e incollare il codice CSS scaricato al suo interno.
4. Aprire l' *App\_Start/bundle. config* e modificare *bootstrap. CSS* in *bootstrap-Theme. CSS*.

Eseguire nuovamente il progetto e l'applicazione ha un nuovo aspetto. La figura seguente mostra l'effetto del tema Amelia:

![Tema di bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Sono disponibili molti temi bootstrap, sia versioni gratuite che Premium. Bootstrap offre inoltre un'ampia gamma di componenti dell'interfaccia utente, ad esempio [elenchi a discesa](http://twitter.github.io/bootstrap/components.html#dropdowns), [gruppi di pulsanti](http://twitter.github.io/bootstrap/components.html#buttonGroups)e [Icone](http://twitter.github.io/bootstrap/base-css.html#images). Per ulteriori informazioni sul bootstrap, vedere [il sito bootstrap](http://twitter.github.io/bootstrap/).

Se si usa progettazione Web Form in Visual Studio, si noti che la finestra di progettazione non supporta CSS3, quindi non Mostra in modo accurato tutti gli effetti dei temi bootstrap o le modifiche di layout reattive. Tuttavia, le pagine Web Form vengono visualizzate correttamente quando vengono visualizzate con un browser.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Aggiunta del supporto per Framework aggiuntivi

Quando si seleziona un modello, la casella di controllo per i Framework utilizzati dal modello viene selezionata automaticamente. Se ad esempio si seleziona il modello **Web Form** , la casella di controllo **Web Form** è selezionata e non è possibile cancellarla.

![Seleziona un modello](creating-web-projects-in-visual-studio/_static/image21.png)

![Aggiungi Framework](creating-web-projects-in-visual-studio/_static/image22.png)

È possibile selezionare la casella di controllo per un Framework che non è incluso nel modello per aggiungere il supporto per tale framework quando viene creato il progetto. Ad esempio, per abilitare l'utilizzo delle pagine Web Forms *. aspx* dopo aver selezionato il modello MVC, selezionare la casella di controllo **Web Form** . In alternativa, per abilitare MVC quando si usa il modello Web Form, fare clic sulla casella di controllo **MVC** . L'aggiunta di un Framework consente il supporto in fase di progettazione e di Runtime. Se ad esempio si aggiunge il supporto MVC a un progetto Web Form, sarà possibile eseguire il patibolo dei controller e delle visualizzazioni.

Se si combinano Web Form e MVC in un progetto e si abilitano [URL brevi](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Form, potrebbe verificarsi un problema di routing imprevisto in cui un URL ha più destinazioni possibili. Le route definite per prime avranno la precedenza. Se, ad esempio, si dispone di un `Home` controller e di una pagina *Home. aspx* , l'url del `http://contoso.com/home` passerà a *Home. aspx* se si chiama il metodo di `EnableFriendlyUrls` prima di chiamare il metodo `MapRoute` in *RouteConfig.cs*oppure lo stesso URL passerà alla visualizzazione predefinita per il controller `Home` se si chiama `MapRoute` prima di `EnableFriendlyUrls`.

L'aggiunta di un Framework non comporta l'aggiunta di alcuna funzionalità di applicazione di esempio. Se ad esempio si aggiunge il supporto per Web Form dopo aver selezionato il modello MVC, non verrà creato alcun file *default. aspx* Home page. Vengono aggiunte solo le cartelle, i file e i riferimenti necessari per supportare il Framework. Per questo motivo, l'aggiunta di Framework non modifica le opzioni di autenticazione, implementate dal codice nelle applicazioni di esempio create dai modelli. Se ad esempio si seleziona il modello vuoto e si aggiunge il supporto per Web Form o MVC, il pulsante **Configura autenticazione** sarà disabilitato.

Le sezioni seguenti descrivono brevemente l'effetto di ogni casella di controllo.

### <a name="add-web-forms-support"></a>Aggiunta del supporto per Web Form

Crea *app vuote\_* le cartelle di dati e *modelli* e un file *Global. asax* . Sono già stati creati da tutti i modelli diversi dal modello vuoto, quindi la selezione della casella di controllo Web Form non fa alcuna differenza per altri modelli.

Il modello Web Form Abilita URL brevi per impostazione predefinita, ma quando si aggiunge il supporto di Web Form ad altri modelli selezionando gli URL descrittivi della casella di controllo Web Form non vengono abilitati automaticamente.

### <a name="add-mvc-support"></a>Aggiungi supporto MVC

Installa i pacchetti NuGet MVC, Razor e pagine Web, Crea app vuote *\_dati*, *controller*, *modelli*e cartelle *viste* , crea un' *app\_* cartella di avvio con il file *RouteConfig.cs* e crea il file *Global. asax* .

### <a name="add-web-api-support"></a>Aggiunta del supporto per l'API Web

Installa i pacchetti NuGet WebApi e Newtonsoft. JSON, crea cartelle di *dati\_app*vuote, *controller*e *modelli* , crea un' *app\_* cartella di avvio con il file *WebApiConfig.cs* e crea il file *Global. asax* .

<a id="auth"></a>
## <a name="authentication-methods"></a>Metodi di autenticazione

Visual Studio 2013 offre diverse opzioni di autenticazione per i modelli Web Form, MVC e API Web:

- [Nessuna autenticazione](#noauth)
- [Singoli account utente](#indauth) (ASP.NET Identity, noto in precedenza come appartenenza ASP.NET)
- [Account aziendali](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticazione di Windows](#winauth) (Intranet)

![Finestra di dialogo Configura autenticazione](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Nessuna autenticazione

Se si seleziona **Nessuna autenticazione**, l'applicazione di esempio non conterrà alcuna pagina Web per l'accesso, nessuna interfaccia utente che indichi chi è connesso, nessuna classe di entità per un database di appartenenza e nessuna stringa di connessione per un database di appartenenza.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Account utente singoli

Se si selezionano **singoli account utente**, l'applicazione di esempio verrà configurata in modo da utilizzare ASP.NET Identity (precedentemente nota come appartenenza ASP.NET) per l'autenticazione utente. ASP.NET Identity consente a un utente di registrare un account creando un nome utente e una password nel sito oppure effettuando l'accesso con i provider di social networking, ad esempio Facebook, Google, account Microsoft o Twitter. L'archivio dati predefinito per i profili utente in ASP.NET Identity è un database SQL Server database locale, che è possibile distribuire in SQL Server o nel database SQL di Azure per il sito di produzione.

In Visual Studio 2013 queste funzionalità sono identiche a quelle di Visual Studio 2012, ma il codice sottostante per il sistema di appartenenze ASP.NET è stato riscritto. I vantaggi della nuova codebase sono i seguenti:

- Il nuovo sistema di appartenenza si basa su [OWIN](http://owin.org/) anziché sul modulo ASP.NET Forms Authentication. Ciò significa che è possibile usare lo stesso meccanismo di autenticazione se si usa Web Form o MVC in IIS oppure se si sta eseguendo l'hosting self-service di API Web o SignalR.
- Il nuovo database di appartenenza viene gestito da Entity Framework Code First e tutte le tabelle sono rappresentate dalle classi di entità che è possibile modificare. Ciò significa che è possibile personalizzare facilmente lo schema del database e l'interfaccia utente Web correlata al profilo per adattarla alle proprie esigenze ed è possibile distribuire facilmente gli aggiornamenti usando Migrazioni Code First.

Il nuovo sistema di appartenenza viene implementato automaticamente nei nuovi modelli e può essere implementato manualmente in qualsiasi progetto destinato a .NET 4,5 o versione successiva.

ASP.NET Identity è una scelta ottimale se si crea un sito Web Internet che è principalmente per i clienti esterni. Se l'organizzazione utilizza Active Directory o Office 365 e si desidera creare un progetto che Abilita l'accesso Single Sign-on per i dipendenti e i partner aziendali, l'opzione **account aziendali** potrebbe essere una scelta migliore.

Per ulteriori informazioni sull'opzione account utente singoli, vedere le risorse seguenti:

- [www.asp.net/identity](../../../identity/index.md). Documentazione su ASP.NET Identity nel sito Web ASP.NET.
- [Creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Viene inoltre illustrato come personalizzare i dati del profilo utente.
- [API Web-servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md)
- [Aggiunta di account di accesso esterni all'applicazione ASP.NET in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Account aziendali

Se si seleziona **account aziendali**, l'applicazione di esempio verrà configurata per l'utilizzo di Windows Identity Foundation (WIF) per l'autenticazione basata sugli account utente in Azure Active Directory (Azure ad, che include Office 365) o Active Directory di Windows Server. Per ulteriori informazioni, vedere [Opzioni di autenticazione degli account aziendali](#orgauthoptions) più avanti in questo argomento.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticazione di Windows

Se si seleziona **l'autenticazione di Windows**, l'applicazione di esempio verrà configurata per l'utilizzo del modulo IIS di autenticazione di Windows per l'autenticazione. L'applicazione visualizzerà il dominio e l'ID utente dell'account di Active Directory o del computer locale che è stato registrato in Windows, ma non includerà l'interfaccia utente di registrazione o di accesso. Questa opzione è destinata ai siti Web Intranet.

In alternativa, è possibile creare un sito Intranet che usa l'autenticazione di Active Directory scegliendo l' [opzione locale in account aziendali](#orgauthonprem). L'opzione locale usa Windows Identity Foundation (WIF) invece del modulo di autenticazione di Windows. Per configurare l'opzione locale sono necessari alcuni passaggi aggiuntivi, ma WIF Abilita le funzionalità che non sono disponibili con il modulo di autenticazione di Windows. Con WIF, ad esempio, è possibile configurare l'accesso all'applicazione in Active Directory ed eseguire query sui dati della directory.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opzioni di autenticazione dell'account aziendale

La finestra di dialogo **Configura autenticazione** offre diverse opzioni per Azure Active Directory (Azure ad, che include Office 365) o l'autenticazione dell'account Windows Server Active Directory (ad):

- [Organizzazione cloud-singola](#orgauthsingle) (Azure ad o ad usando l'integrazione di directory con Azure ad)
- [Cloud-più organizzazioni](#orgauthmulti) (Azure ad o ad usando l'integrazione di directory con Azure ad)
- [Locale](#orgauthonprem) (ad)

Se si vuole provare una delle opzioni di Azure AD ma non si ha ancora un account, [fare clic qui per iscriversi per ottenere un account di Azure ad](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se si sceglie una delle opzioni Azure AD, il progetto richiede un database ed è necessario accedere a un account amministratore globale per il tenant di Azure AD. Immettere il nome e la password di un account aziendale, ad esempio admin@contoso.onmicrosoft.com, che disponga di autorizzazioni amministrative per il tenant del Azure AD.
> 
> **Non immettere le credenziali per un account Microsoft, ad esempio contoso@hotmail.com, nella finestra di dialogo di accesso.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Autenticazione cloud-singola organizzazione

![Autenticazione a organizzazione singola](creating-web-projects-in-visual-studio/_static/image24.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in un [tenant](https://technet.microsoft.com/library/jj573650.aspx)Azure ad. Ad esempio, il sito è contoso.com e verrà reso disponibile ai dipendenti della società Contoso che si trovano nel tenant di contoso.onmicrosoft.com. Non sarà possibile configurare Azure AD per consentire agli utenti di altri tenant di accedere all'applicazione.

#### <a name="domain"></a>Dominio

Immettere il dominio Azure AD in cui si desidera configurare l'applicazione, ad esempio: `contoso.onmicrosoft.com`. Se si dispone di un [dominio personalizzato](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), ad esempio `contoso.com` anziché `contoso.onmicrosoft.com`, è possibile immetterlo qui.

#### <a name="access-level"></a>Livello di accesso

Se l'applicazione deve eseguire una query o aggiornare le informazioni della directory usando il API Graph, scegliere **Single Sign-on, lettura dati directory** o **Single Sign-on, lettura e scrittura dati directory**. In caso contrario, scegliere **Single Sign-on**. Per ulteriori informazioni, vedere [livelli di accesso alle applicazioni](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [utilizzo del API Graph per eseguire query Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI ID applicazione

Per impostazione predefinita, il modello crea automaticamente un URI ID applicazione aggiungendo il nome del progetto al dominio Azure AD. Se, ad esempio, il nome del progetto è `Example` e il dominio è `contoso.onmicrosoft.com`, l'URI dell'ID applicazione diventa `https://contoso.onmicrosoft.com/Example`. Se si vuole specificare manualmente l'URI dell'ID applicazione, espandere la sezione **altre opzioni** e immettere l'URI dell'ID applicazione nella casella di testo. L'URI dell'ID applicazione deve iniziare con `https://`.

Per impostazione predefinita, se un'applicazione già sottoposta a provisioning in Azure AD ha lo stesso URI dell'ID applicazione di quello utilizzato da Visual Studio per il progetto, il progetto verrà connesso all'applicazione esistente anziché eseguire il provisioning di un nuovo. Se si desidera che venga effettuato il provisioning di una nuova applicazione, deselezionare la casella di controllo **Sovrascrivi la voce dell'applicazione se ne esiste già uno con lo stesso ID** .

Se la casella di controllo **Sovrascrivi** è deselezionata e Visual Studio rileva un'applicazione esistente con lo stesso URI ID applicazione, crea un nuovo URI aggiungendo un numero all'URI che avrebbe utilizzato. Si supponga, ad esempio, che il nome del progetto sia `Example`, si lasci vuota la casella di testo, si deseleziona la casella di controllo **Sovrascrivi** e il tenant di Azure ad dispone già di un'applicazione con l'URI `https://contoso.onmicrosoft.com/Example`. In tal caso, verrà eseguito il provisioning di una nuova applicazione con un URI ID applicazione come `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisioning dell'applicazione in Azure AD

Per eseguire il provisioning dell'applicazione in Azure AD o connettere il progetto a un'applicazione esistente, Visual Studio richiede le credenziali di un amministratore globale per il dominio. Quando si fa clic su **OK** nella finestra di dialogo **Configura autenticazione** , vengono richiesti il nome utente e la password di un amministratore globale per il dominio specificato. In seguito, quando si fa clic su **Crea progetto** nella finestra di dialogo **nuovo progetto ASP.NET** , Visual Studio esegue il provisioning dell'applicazione in Azure ad. Si noti che nell'ambito di questo processo Visual Studio incorpora i valori del segreto client nel file Web. config che scadono un anno dopo la creazione.

Per informazioni su come creare applicazioni che usano l'autenticazione **cloud-Single Organization** , vedere le risorse seguenti:

- [Autenticazione di Azure](../2012/windows-azure-authentication.md)
- [Aggiunta del processo di accesso nell'applicazione Web tramite Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Sviluppo di app ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteggere API Web ASP.NET con i componenti Azure AD e Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Le esercitazioni non sono ancora state aggiornate per Visual Studio 2013; alcune delle esercitazioni dirette manualmente sono automatizzate in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Autenticazione cloud-più organizzazioni

![Autenticazione a più organizzazioni](creating-web-projects-in-visual-studio/_static/image25.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in più [tenant](https://technet.microsoft.com/library/jj573650.aspx)Azure ad. Ad esempio, il sito è contoso.com e verrà reso disponibile ai dipendenti della società Contoso che si trovano nel tenant contoso.onmicrosoft.com e dipendenti della società Fabrikam che si trovano nel tenant fabrikam.onmicrosoft.com.

Le impostazioni immesse e il passaggio di provisioning dell'applicazione sono simili a quelle dell' [autenticazione a singola organizzazione](#orgauthsingle).

Per informazioni su come creare applicazioni che usano l'autenticazione **cloud-multiorganization** , vedere le risorse seguenti:

- [Facile integrazione di app Web con Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) nel Blog del Team di Active Directory.
- [Sviluppo di applicazioni Web multi-tenant con Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) esercitazione. L'esercitazione non è ancora stata aggiornata per Visual Studio 2013; una parte di ciò che l'esercitazione indirizza manualmente è automatizzata in Visual Studio 2013.
- [Prima di poter accedere, è necessario iscriversi con l'App ASP.NET per più organizzazioni](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog di Vittorio Bertocci che spiega come risolvere un problema comune riscontrato dagli utenti durante la creazione di un progetto che usa l'autenticazione a più organizzazioni.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticazione organizzativa locale

![Autenticazione organizzativa locale](creating-web-projects-in-visual-studio/_static/image26.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Windows Server Active Directory (AD) e non si desidera utilizzare Azure AD. È possibile utilizzare questa opzione per creare un sito Intranet o un sito Internet. Per un sito Internet, utilizzare Active Directory Federation Services (ADFS) per fornire l'accesso ad Active Directory. Per altre informazioni, vedere [usare l'opzione di autenticazione organizzativa locale (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Per un sito Intranet, in alternativa è possibile scegliere [l'autenticazione di Windows](#winauth) anziché questa opzione. Per l'opzione autenticazione di Windows non è necessario fornire un URL del documento di metadati. Tuttavia, l'autenticazione di Windows non offre la possibilità di controllare l'accesso alle applicazioni in Active Directory o di eseguire query sui dati della directory.

#### <a name="on-premises-authority"></a>Autorità locale

Immettere un URL che punti al documento di metadati. Il documento di metadati contiene le coordinate dell'autorità. Le coordinate verranno utilizzate dall'applicazione per guidare il flusso di accesso Web.

#### <a name="application-id-uri"></a>URI ID applicazione

Specificare un URI univoco che può essere usato da AD per identificare l'applicazione oppure lasciare vuoto per consentire a Visual Studio di crearne uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

In questo documento sono state fornite alcune informazioni di base per la creazione di un nuovo progetto Web ASP.NET in Visual Studio 2013. Per ulteriori informazioni sull'utilizzo di per Visual Studio per lo sviluppo Web, vedere [https://www.asp.net/visual-studio/](../../index.md).
