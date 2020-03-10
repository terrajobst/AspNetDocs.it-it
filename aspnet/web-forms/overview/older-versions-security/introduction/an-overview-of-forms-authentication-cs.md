---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Panoramica dell'autenticazione basata su formC#() | Microsoft Docs
author: rick-anderson
description: Creazione di route personalizzate
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 009c3f84e00d648ede4a15e530ceac2d23e01eec
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546431"
---
# <a name="an-overview-of-forms-authentication-c"></a>Panoramica dell'autenticazione basata su formC#()

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> In questa esercitazione si passerà dalla semplice discussione all'implementazione; in particolare, si esaminerà l'implementazione dell'autenticazione basata su form. L'applicazione Web che inizia a costruire in questa esercitazione continuerà a essere compilata nelle esercitazioni successive, in quanto si passa dall'autenticazione basata su form all'appartenenza e ai ruoli.
> 
> Per altre informazioni su questo argomento, vedere l'articolo relativo all' [uso dell'autenticazione basata su form di base in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](security-basics-and-asp-net-support-cs.md) sono state illustrate le varie opzioni di autenticazione, autorizzazione e account utente fornite da ASP.NET. In questa esercitazione si passerà dalla semplice discussione all'implementazione; in particolare, si esaminerà l'implementazione dell'autenticazione basata su form. L'applicazione Web che inizia a costruire in questa esercitazione continuerà a essere compilata nelle esercitazioni successive, in quanto si passa dall'autenticazione basata su form all'appartenenza e ai ruoli.

Questa esercitazione inizia con una panoramica approfondita del flusso di lavoro di autenticazione basata su form, un argomento che è stato toccato nell'esercitazione precedente. In seguito, si creerà un sito Web ASP.NET tramite il quale dimostrando i concetti di autenticazione basata su form. Si configurerà quindi il sito per l'uso dell'autenticazione basata su form, si creerà una semplice pagina di accesso e si vedrà come determinare, nel codice, se un utente è autenticato e, in caso affermativo, il nome utente con cui è stato eseguito l'accesso.

Comprendere il flusso di lavoro di autenticazione basata su form, abilitarlo in un'applicazione Web e creare le pagine di accesso e di disconnessione sono tutti passaggi fondamentali per la creazione di un'applicazione ASP.NET che supporta gli account utente e autentica gli utenti tramite una pagina Web. Per questo motivo, e poiché queste esercitazioni si basano su un altro, si consiglia di eseguire questa esercitazione in modo completo prima di passare a quella successiva anche se si è già verificata la configurazione dell'autenticazione basata su form nei progetti precedenti.

## <a name="understanding-the-forms-authentication-workflow"></a>Informazioni sul flusso di lavoro di autenticazione basata su form

Quando il runtime ASP.NET elabora una richiesta per una risorsa ASP.NET, ad esempio una pagina ASP.NET o un servizio Web ASP.NET, la richiesta genera un certo numero di eventi durante il ciclo di vita. Ci sono eventi generati all'inizio e alla fine della richiesta, quelli generati quando la richiesta viene autenticata e autorizzata, un evento generato in caso di eccezione non gestita e così via. Per visualizzare un elenco completo degli eventi, vedere gli eventi dell' [oggetto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

I *moduli HTTP* sono classi gestite il cui codice viene eseguito in risposta a un evento specifico nel ciclo di vita della richiesta. ASP.NET viene fornito con una serie di moduli HTTP che eseguono attività essenziali dietro le quinte. Due moduli HTTP incorporati che sono particolarmente rilevanti per la nostra discussione sono:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** : autentica l'utente controllando il ticket di autenticazione basata su form, che in genere è incluso nella raccolta di cookie dell'utente. Se non è presente alcun ticket di autenticazione basata su form, l'utente è anonimo.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** : determina se l'utente corrente è autorizzato o meno ad accedere all'URL richiesto. Questo modulo determina l'autorità consultando le regole di autorizzazione specificate nei file di configurazione dell'applicazione. ASP.NET include anche la [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) che determina l'autorità consultando gli ACL dei file richiesti.

Il `FormsAuthenticationModule` tenta di autenticare l'utente prima dell'esecuzione di `UrlAuthorizationModule` (e `FileAuthorizationModule`). Se l'utente che effettua la richiesta non è autorizzato ad accedere alla risorsa richiesta, il modulo di autorizzazione termina la richiesta e restituisce uno stato [HTTP 401 non autorizzato](http://www.checkupdown.com/status/E401.html) . Negli scenari di autenticazione di Windows, lo stato HTTP 401 viene restituito al browser. Questo codice di stato fa sì che il browser chieda all'utente le credenziali tramite una finestra di dialogo modale. Con l'autenticazione basata su form, tuttavia, lo stato HTTP 401 non autorizzato viene mai inviato al browser perché FormsAuthenticationModule rileva questo stato e lo modifica per reindirizzare l'utente alla pagina di accesso (tramite uno stato di [reindirizzamento http 302](http://www.checkupdown.com/status/E302.html) ).

La responsabilità della pagina di accesso è determinare se le credenziali dell'utente sono valide e, in caso affermativo, creare un ticket di autenticazione basata su form e reindirizzare l'utente alla pagina che stava tentando di visitare. Il ticket di autenticazione è incluso nelle richieste successive alle pagine del sito Web, che il `FormsAuthenticationModule` utilizza per identificare l'utente.

![Flusso di lavoro di autenticazione basata su form](an-overview-of-forms-authentication-cs/_static/image1.png)

**Figura 1**: flusso di lavoro di autenticazione basata su form

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Memorizzazione del ticket di autenticazione nelle visite di pagina

Dopo aver eseguito l'accesso, il ticket di autenticazione basata su form deve essere inviato di nuovo al server Web per ogni richiesta, in modo che l'utente rimanga connesso mentre Esplora il sito. Questa operazione viene in genere eseguita inserendo il ticket di autenticazione nella raccolta di cookie dell'utente. I [cookie](http://en.wikipedia.org/wiki/HTTP_cookie) sono piccoli file di testo che risiedono nel computer dell'utente e vengono trasmessi nelle intestazioni HTTP di ogni richiesta al sito Web che ha creato il cookie. Pertanto, una volta che il ticket di autenticazione basata su form è stato creato e archiviato nei cookie del browser, ogni visita successiva al sito invia il ticket di autenticazione insieme alla richiesta, identificando quindi l'utente.

Un aspetto dei cookie è la scadenza, ovvero la data e l'ora in cui il browser elimina il cookie. Quando il cookie di autenticazione basata su form scade, l'utente non può più essere autenticato e quindi diventa anonimo. Quando un utente visita da un terminale pubblico, è probabile che voglia che il ticket di autenticazione scada quando chiude il browser. Quando si visita da casa, tuttavia, lo stesso utente potrebbe voler ricordare il ticket di autenticazione tra i riavvii del browser in modo che non debbano eseguire nuovamente l'accesso ogni volta che visitano il sito. Questa decisione viene spesso eseguita dall'utente sotto forma di casella di controllo "memorizza me" nella pagina di accesso. Nel passaggio 3 si esaminerà come implementare una casella di controllo "Remember me" nella pagina di accesso. L'esercitazione seguente illustra in dettaglio le impostazioni di timeout dei ticket di autenticazione.

> [!NOTE]
> È possibile che l'agente utente usato per accedere al sito Web non supporti i cookie. In tal caso, ASP.NET può usare ticket di autenticazione basata su form senza cookie. In questa modalità, il ticket di autenticazione è codificato nell'URL. Verranno esaminati i ticket di autenticazione senza cookie e il modo in cui vengono creati e gestiti nell'esercitazione successiva.

### <a name="the-scope-of-forms-authentication"></a>Ambito dell'autenticazione basata su form

Il `FormsAuthenticationModule` è codice gestito che fa parte del runtime di ASP.NET. Prima della versione 7 del server Web di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) , esisteva una barriera distinta tra la pipeline HTTP di IIS e la pipeline del runtime di ASP.NET. In breve, in IIS 6 e versioni precedenti, il `FormsAuthenticationModule` viene eseguito solo quando una richiesta viene delegata da IIS al runtime di ASP.NET. Per impostazione predefinita, IIS elabora il contenuto statico stesso, ad esempio pagine HTML e file CSS e di immagine, e invia le richieste al runtime ASP.NET solo quando viene richiesta una pagina con estensione aspx, asmx o ashx.

IIS 7, tuttavia, consente le pipeline IIS e ASP.NET integrate. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per richiamare il FormsAuthenticationModule per *tutte* le richieste. Inoltre, con IIS 7 è possibile definire le regole di autorizzazione dell'URL per i file di qualsiasi tipo. Per ulteriori informazioni, vedere la pagina relativa alle [modifiche tra IIS6 e IIS7 Security](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [la sicurezza della piattaforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)e [informazioni sull'autorizzazione dell'URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Long Story Short, nelle versioni precedenti a IIS 7, è possibile usare solo l'autenticazione basata su form per proteggere le risorse gestite dal runtime di ASP.NET. Analogamente, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime di ASP.NET. Con IIS 7 è tuttavia possibile integrare FormsAuthenticationModule e UrlAuthorizationModule nella pipeline HTTP di IIS, estendendo così questa funzionalità a tutte le richieste.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Passaggio 1: creazione di un sito Web ASP.NET per questa serie di esercitazioni

Per raggiungere il maggior pubblico possibile, il sito Web ASP.NET che verrà creato in tutta la serie verrà creato con la versione gratuita di Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)di Microsoft. Il `SqlMembershipProvider` archivio utente viene implementato in un database [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) . Se si usa Visual Studio 2005 o un'edizione diversa di Visual Studio 2008 o SQL Server, i passaggi saranno quasi identici e verranno sottolineate eventuali differenze non semplici.

> [!NOTE]
> L'applicazione Web demo usata in ogni esercitazione è disponibile come download. Questa applicazione scaricabile è stata creata con Visual Web Developer 2008 come destinazione per la .NET Framework versione 3,5. Poiché l'applicazione è destinata a .NET 3,5, il relativo file Web. config include elementi di configurazione aggiuntivi specifici di 3,5. Long Story Short, se è ancora necessario installare .NET 3,5 nel computer, l'applicazione Web scaricabile non funzionerà senza prima rimuovere il markup specifico di 3,5 da Web. config.

Prima di poter configurare l'autenticazione basata su form, è necessario prima di tutto un sito Web ASP.NET. Per iniziare, creare un nuovo sito Web ASP.NET basato su file system. A tale scopo, avviare Visual Web Developer, quindi scegliere nuovo sito Web dal menu file, visualizzando la finestra di dialogo nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco a discesa percorso su file System, scegliere una cartella in cui collocare il sito Web e impostare il linguaggio su C#. Verrà creato un nuovo sito Web con una pagina di ASP.NET default. aspx, una cartella di dati dell'app\_e un file Web. config.

> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. I progetti di siti Web non dispongono di un file di progetto, mentre i progetti di applicazioni Web simulano l'architettura del progetto in Visual Studio .NET 2002/2003: includono un file di progetto e compilano il codice sorgente del progetto in un unico assembly, inserito nella cartella/bin. Visual Studio 2005 inizialmente solo i progetti di siti Web supportati, sebbene il modello di progetto applicazione Web sia stato reintrodotto con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Le edizioni Visual Web Developer 2005 e 2008, tuttavia, supportano solo i progetti di siti Web. Verrà utilizzato il modello di progetto di sito Web. Se invece si utilizza un'edizione non Express e si desidera utilizzare il [modello di progetto di applicazione Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) , è possibile farlo, ma tenere presente che potrebbero esserci alcune discrepanze tra gli elementi visualizzati sullo schermo e i passaggi da eseguire rispetto alle schermate visualizzate e alle istruzioni fornite in queste esercitazioni.

[![creare un nuovo sito Web basato su file System](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Figura 2**: creare un nuovo sito Web basato su file System ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image4.png))

### <a name="adding-a-master-page"></a>Aggiunta di una pagina master

Successivamente, aggiungere una nuova pagina master al sito nella directory radice denominata Site. master. Le [pagine master](https://msdn.microsoft.com/library/wtxbf3hh.aspx) consentono a uno sviluppatore di pagine di definire un modello a livello di sito che può essere applicato alle pagine di ASP.NET. Il vantaggio principale delle pagine master è che l'aspetto complessivo del sito può essere definito in un'unica posizione, semplificando così l'aggiornamento o la modifica del layout del sito.

[![aggiungere una pagina master denominata Site. master al sito Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Figura 3**: aggiungere una pagina master denominata Site. master al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image7.png))

Definire il layout di pagina a livello di sito nella pagina master. È possibile utilizzare la visualizzazione progettazione e aggiungere qualsiasi layout o controllo Web necessario oppure è possibile aggiungere manualmente il markup manualmente nella visualizzazione origine. Ho strutturato il layout della mia pagina master per simulare il layout usato nei miei dati nella serie di esercitazioni di *[ASP.NET 2,0](../../data-access/index.md)* (vedere la figura 4). La pagina master usa i [fogli di stile](http://www.w3schools.com/css/default.asp) CSS per il posizionamento e gli stili con le impostazioni CSS definite nel file Style. CSS, incluso nel download associato a questa esercitazione. Sebbene non sia possibile capire dal markup mostrato di seguito, le regole CSS sono definite in modo tale che la navigazione &lt;contenuto del&gt;div sia assolutamente posizionata in modo che venga visualizzata a sinistra e con una larghezza fissa di 200 pixel.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Una pagina master definisce sia il layout della pagina statica che le aree che possono essere modificate dalle pagine ASP.NET che usano la pagina master. Queste aree modificabili del contenuto sono indicate dal controllo `ContentPlaceHolder`, che può essere visualizzato all'interno del contenuto &lt;&gt;div. La pagina master contiene un solo `ContentPlaceHolder` (MainContent), ma la pagina master potrebbe avere più ContentPlaceHolders.

Con il markup immesso in precedenza, il passare alla visualizzazione progettazione Mostra il layout della pagina master. Le pagine ASP.NET che usano questa pagina master avranno questo layout uniforme, con la possibilità di specificare il markup per l'area `MainContent`.

[![pagina master, quando viene visualizzata tramite la visualizzazione progettazione](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Figura 4**: pagina master, quando viene visualizzata tramite la visualizzazione progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image10.png))

### <a name="creating-content-pages"></a>Creazione di pagine di contenuto

A questo punto è presente una pagina default. aspx nel sito Web, ma non viene usata la pagina master appena creata. Sebbene sia possibile modificare il markup dichiarativo di una pagina Web per utilizzare una pagina master, se la pagina non contiene contenuto, è ancora più semplice eliminare la pagina e aggiungerla nuovamente al progetto, specificando la pagina master da utilizzare. Pertanto, iniziare eliminando default. aspx dal progetto.

Fare quindi clic con il pulsante destro del mouse sul nome del progetto nella Esplora soluzioni e scegliere di aggiungere un nuovo Web Form denominato default. aspx. Questa volta, selezionare la casella di controllo "Seleziona pagina master" e scegliere la pagina master del sito. master dall'elenco.

[![aggiungere una nuova pagina default. aspx scegliendo di selezionare una pagina master](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Figura 5**: aggiungere una nuova pagina default. aspx scegliendo di selezionare una pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image13.png))

![Utilizzare la pagina master site. master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Figura 6**: usare la pagina master site. master

> [!NOTE]
> Se si usa il modello di progetto applicazione Web, la finestra di dialogo Aggiungi nuovo elemento non include una casella di controllo "Seleziona pagina master". È invece necessario aggiungere un elemento di tipo "Web Content form". Dopo aver scelto l'opzione "modulo contenuto Web" e aver fatto clic su Aggiungi, in Visual Studio viene visualizzata la stessa finestra di dialogo Seleziona un master mostrata nella figura 6.

Il markup dichiarativo della pagina default. aspx include solo una direttiva @Page che specifica il percorso del file della pagina master e un controllo contenuto per la MainContent ContentPlaceHolder della pagina master.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Per il momento, lasciare vuoto default. aspx. In questa esercitazione verrà ritornato più avanti in questa esercitazione per aggiungere contenuto.

> [!NOTE]
> La pagina master include una sezione per un menu o un'altra interfaccia di navigazione. Questa interfaccia viene creata in un'esercitazione futura.

## <a name="step-2-enabling-forms-authentication"></a>Passaggio 2: abilitazione dell'autenticazione basata su form

Con il sito Web ASP.NET creato, l'attività successiva consiste nell'abilitare l'autenticazione basata su form. La configurazione di autenticazione dell'applicazione viene specificata tramite l' [elemento`<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx) in Web. config. L'elemento `<authentication>` contiene un singolo attributo denominato mode che specifica il modello di autenticazione usato dall'applicazione. Questo attributo può avere uno dei quattro valori seguenti:

- **Windows** : come illustrato nell'esercitazione precedente, quando un'applicazione usa l'autenticazione di Windows, è responsabilità del server Web autenticare il visitatore e questa operazione viene in genere eseguita tramite l'autenticazione di base, digest o integrata di Windows.
- **Moduli**: gli utenti vengono autenticati tramite un modulo in una pagina Web.
- **Passport**: gli utenti vengono autenticati tramite la rete Passport di Microsoft.
- **None**: non viene utilizzato alcun modello di autenticazione; tutti i visitatori sono anonimi.

Per impostazione predefinita, le applicazioni ASP.NET utilizzano l'autenticazione di Windows. Per modificare il tipo di autenticazione per l'autenticazione basata su form, è necessario modificare l'attributo della modalità dell'elemento `<authentication>` in form.

Se il progetto non contiene ancora un file Web. config, aggiungerne uno facendo clic con il pulsante destro del mouse sul nome del progetto nella Esplora soluzioni, scegliendo Aggiungi nuovo elemento e quindi aggiungendo un file di configurazione Web.

[![se il progetto non include ancora Web. config, aggiungerlo ora](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Figura 7**: se il progetto non include ancora Web. config, aggiungerlo ora ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image17.png))

Individuare quindi l'elemento `<authentication>` e aggiornarlo per utilizzare l'autenticazione basata su form. Dopo questa modifica, il markup del file Web. config dovrebbe essere simile al seguente:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Poiché Web. config è un file XML, la combinazione di maiuscole e minuscole è importante. Assicurarsi di impostare l'attributo mode su Forms, con un valore maiuscolo "F". Se si usa una combinazione di maiuscole e minuscole diversa, ad esempio "Forms", si riceverà un errore di configurazione quando si visita il sito tramite un browser.

L'elemento `<authentication>` può includere facoltativamente un elemento `<forms>` figlio che contiene impostazioni specifiche dell'autenticazione basata su form. Per il momento, è sufficiente usare le impostazioni predefinite di autenticazione basata su form. L'`<forms>` elemento figlio verrà esaminato più dettagliatamente nell'esercitazione successiva.

## <a name="step-3-building-the-login-page"></a>Passaggio 3: compilazione della pagina di accesso

Per supportare l'autenticazione basata su form, il sito Web necessita di una pagina di accesso. Come illustrato nella sezione "informazioni sul flusso di lavoro di autenticazione basata su form", il `FormsAuthenticationModule` reindirizza automaticamente l'utente alla pagina di accesso se tentano di accedere a una pagina che non sono autorizzati a visualizzare. Sono inoltre presenti controlli Web ASP.NET che visualizzeranno un collegamento alla pagina di accesso agli utenti anonimi. Si pone la domanda "Qual è l'URL della pagina di accesso?"

Per impostazione predefinita, il sistema di autenticazione basata su form prevede che la pagina di accesso sia denominata Login. aspx e posizionata nella directory radice dell'applicazione Web. Se si desidera utilizzare un URL diverso della pagina di accesso, è possibile specificarlo nel file Web. config. Nell'esercitazione successiva verrà illustrato come eseguire questa operazione.

La pagina di accesso ha tre responsabilità:

1. Fornire un'interfaccia che consenta al visitatore di immettere le proprie credenziali.
2. Determinare se le credenziali inviate sono valide.
3. "Accedi" all'utente creando il ticket di autenticazione basata su form.

### <a name="creating-the-login-pages-user-interface"></a>Creazione dell'interfaccia utente della pagina di accesso

Si inizia con la prima attività. Aggiungere una nuova pagina ASP.NET alla directory radice del sito denominata Login. aspx e associarla alla pagina master site. master.

[![aggiungere una nuova pagina ASP.NET denominata Login. aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Figura 8**: aggiungere una nuova pagina ASP.NET denominata Login. aspx ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image20.png))

L'interfaccia tipica della pagina di accesso è costituita da due caselle di testo, una per il nome dell'utente, una per la relativa password e un pulsante per l'invio del modulo. I siti Web includono spesso una casella di controllo "memorizza me" che, se selezionata, rende permanente il ticket di autenticazione risultante tra i riavvii del browser.

Aggiungere due caselle di testo a login. aspx e impostare le relative proprietà `ID` rispettivamente su nome utente e password. Impostare anche la proprietà `TextMode` della password su password. Successivamente, aggiungere un controllo CheckBox, impostarne la proprietà `ID` su RememberMe e la relativa proprietà `Text` su "Remember me". Successivamente, aggiungere un pulsante denominato LoginButton la cui proprietà `Text` è impostata su "login". Infine, aggiungere un controllo Web Label e impostare la relativa proprietà `ID` su InvalidCredentialsMessage, la relativa proprietà `Text` su "il nome utente o la password non è valida. Riprovare. ", la relativa proprietà `ForeColor` su rosso e la relativa proprietà `Visible` su false.

A questo punto la schermata dovrebbe essere simile alla schermata nella figura 9 e la sintassi dichiarativa della pagina dovrebbe essere simile alla seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]

[![la pagina di accesso contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Figura 9**: la pagina di accesso contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image23.png))

Infine, creare un gestore eventi per l'evento click del LoginButton. Dalla finestra di progettazione, è sufficiente fare doppio clic sul controllo Button per creare questo gestore eventi.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinare se le credenziali specificate sono valide

È ora necessario implementare l'attività 2 nel gestore dell'evento click del pulsante, che determina se le credenziali specificate sono valide. Per eseguire questa operazione, è necessario essere un archivio utente che contenga tutte le credenziali degli utenti, in modo da poter determinare se le credenziali fornite corrispondono con le credenziali note.

Prima di ASP.NET 2,0, gli sviluppatori erano responsabili dell'implementazione dei propri archivi utente e della scrittura del codice per convalidare le credenziali fornite nell'archivio. La maggior parte degli sviluppatori implementa l'archivio utenti in un database, creando una tabella denominata Users con colonne quali UserName, password, email, LastLoginDate e così via. Questa tabella, quindi, avrà un record per ogni account utente. La verifica delle credenziali fornite da un utente comporterebbe l'esecuzione di una query sul database per un nome utente corrispondente e quindi la verifica che la password nel database corrisponda alla password fornita.

Con ASP.NET 2,0, gli sviluppatori devono usare uno dei provider di appartenenze per gestire l'archivio utenti. In questa serie di esercitazioni verrà usato l'oggetto SqlMembershipProvider, che usa un database SQL Server per l'archivio utente. Quando si utilizza l'oggetto SqlMembershipProvider è necessario implementare uno schema di database specifico che includa le tabelle, le viste e le stored procedure previste dal provider. Si esaminerà come implementare questo schema nell'esercitazione ***creazione dello schema di appartenenza in SQL Server*** . Con il provider di appartenenze sul posto, la convalida delle credenziali dell'utente è semplice quanto la chiamata al [metodo ValidateUser (*username*, *password*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)della [classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx), che restituisce un valore booleano che indica se la validità della combinazione di *nome utente* e *password* . Visto che non è ancora stato implementato l'archivio utenti dell'oggetto SqlMembershipProvider, al momento non è possibile usare il metodo ValidateUser della classe di appartenenza.

Anziché dedicare tempo alla creazione di una tabella di database personalizzata per gli utenti (che sarebbe obsoleta una volta implementato l'oggetto SqlMembershipProvider), è possibile invece impostare come hardcoded le credenziali valide all'interno della pagina di accesso. Nel gestore dell'evento Click di LoginButton aggiungere il codice seguente:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Come si può notare, sono disponibili tre account utente validi: Scott, Jisun e Sam e tutti e tre hanno la stessa password ("password"). Il codice scorre gli array di utenti e password che cercano una corrispondenza valida per nome utente e password. Se il nome utente e la password sono entrambi validi, è necessario eseguire l'accesso all'utente e quindi reindirizzarli alla pagina appropriata. Se le credenziali non sono valide, viene visualizzata l'etichetta InvalidCredentialsMessage.

Quando un utente immette credenziali valide, ho indicato che vengono quindi reindirizzate alla "pagina appropriata". Qual è la pagina appropriata? Tenere presente che quando un utente visita una pagina che non è autorizzato a visualizzare, il FormsAuthenticationModule li reindirizza automaticamente alla pagina di accesso. In questo modo, include l'URL richiesto in QueryString tramite il parametro ReturnUrl. Ovvero, se un utente ha provato a visitare ProtectedPage. aspx e non è stato autorizzato a eseguire questa operazione, il FormsAuthenticationModule li reindirizza a:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Dopo aver eseguito correttamente l'accesso, l'utente deve essere reindirizzato a ProtectedPage. aspx. In alternativa, gli utenti possono visitare la pagina di accesso per la propria volontà. In tal caso, dopo l'accesso l'utente deve essere inviato alla pagina default. aspx della cartella radice.

### <a name="logging-in-the-user"></a>Registrazione nell'utente

Supponendo che le credenziali specificate siano valide, è necessario creare un ticket di autenticazione basata su form, in modo che l'utente acceda al sito. La [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) nello [spazio dei nomi System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) fornisce metodi assortiti per l'accesso e la disconnessione degli utenti tramite il sistema di autenticazione basata su form. Sebbene esistano diversi metodi nella classe FormsAuthentication, i tre a cui si è interessati sono:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) : crea un ticket di autenticazione basata su form per il nome *utente*specificato. Successivamente, questo metodo crea e restituisce un oggetto HttpCookie che contiene il contenuto del ticket di autenticazione. Se *persistCookie* è true, viene creato un cookie permanente.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) : chiama il metodo GetAuthCookie (*username*, *persistCookie*) per generare il cookie di autenticazione basata su form. Questo metodo aggiunge quindi il cookie restituito da GetAuthCookie alla raccolta di cookie (presupponendo che venga utilizzata l'autenticazione basata su form dei cookie; in caso contrario, questo metodo chiama una classe interna che gestisce la logica del ticket senza cookie).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) : questo metodo chiama SetAuthCookie (*username*, *persistCookie*) e quindi reindirizza l'utente alla pagina appropriata.

GetAuthCookie è utile quando è necessario modificare il ticket di autenticazione prima di scrivere il cookie nella raccolta dei cookie. SetAuthCookie è utile se si desidera creare il ticket di autenticazione basata su form e aggiungerlo alla raccolta dei cookie, ma non si desidera reindirizzare l'utente alla pagina appropriata. Potrebbe essere necessario mantenerli nella pagina di accesso o inviarli ad alcune pagine alternative.

Poiché si desidera accedere all'utente e reindirizzarli alla pagina appropriata, utilizzare RedirectFromLoginPage. Aggiornare il gestore dell'evento Click di LoginButton, sostituendo le due righe TODO commentate con la riga di codice seguente:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked);

Quando si crea il ticket di autenticazione basata su form, si usa la proprietà Text della casella di testo UserName per il parametro *nome utente* del ticket di autenticazione basata su form e lo stato di selezione della casella di controllo RememberMe per il parametro *persistCookie* .

Per eseguire il test della pagina di accesso, visitarla in un browser. Per iniziare, immettere credenziali non valide, ad esempio il nome utente "No" e la password "Wrong". Quando si fa clic sul pulsante di accesso, si verificherà un postback e verrà visualizzata l'etichetta InvalidCredentialsMessage.

[![viene visualizzata l'etichetta InvalidCredentialsMessage quando si immettono credenziali non valide](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Figura 10**: l'etichetta InvalidCredentialsMessage viene visualizzata quando si immettono credenziali non valide ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image26.png))

Immettere quindi le credenziali valide e fare clic sul pulsante di accesso. Questa volta, quando si verifica il postback, viene creato un ticket di autenticazione basata su form e si viene reindirizzati automaticamente a default. aspx. A questo punto è stato effettuato l'accesso al sito Web, anche se non sono disponibili segnali visivi per indicare che l'utente è attualmente connesso. Nel passaggio 4 verrà illustrato come determinare a livello di codice se un utente è connesso o meno e come identificare l'utente che visita la pagina.

Il passaggio 5 esamina le tecniche per la registrazione di un utente dal sito Web.

### <a name="securing-the-login-page"></a>Protezione della pagina di accesso

Quando l'utente immette le proprie credenziali e invia il modulo della pagina di accesso, le credenziali, inclusa la password, vengono trasmesse tramite Internet al server Web come *testo normale*. Ciò significa che qualsiasi hacker che analizza il traffico di rete può visualizzare il nome utente e la password. Per evitare questo problema, è fondamentale crittografare il traffico di rete tramite [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). In questo modo, le credenziali, nonché il markup HTML dell'intera pagina, verranno crittografate dal momento in cui lasciano il browser fino a quando non vengono ricevute dal server Web.

A meno che il sito Web non contenga informazioni riservate, sarà necessario utilizzare SSL solo nella pagina di accesso e in altre pagine in cui la password dell'utente viene altrimenti inviata in rete in testo normale. Non è necessario preoccuparsi di proteggere il ticket di autenticazione basata su form poiché, per impostazione predefinita, è crittografato e con firma digitale (per evitare manomissioni). Nell'esercitazione seguente viene presentata una discussione più approfondita sulla sicurezza dei ticket di autenticazione basata su form.

> [!NOTE]
> Molti siti web finanziari e medici sono configurati per l'uso di SSL in *tutte le* pagine accessibili agli utenti autenticati. Se si compila un sito Web di questo tipo, è possibile configurare il sistema di autenticazione basata su form in modo che il ticket di autenticazione basata su form venga trasmesso solo tramite una connessione protetta. Si esamineranno le varie opzioni di configurazione dell'autenticazione basata su form nell'esercitazione successiva, la *[configurazione dell'autenticazione basata su form e gli argomenti avanzati](forms-authentication-configuration-and-advanced-topics-cs.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Passaggio 4: rilevamento dei visitatori autenticati e determinazione della relativa identità

A questo punto è stata abilitata l'autenticazione basata su form e è stata creata una pagina di accesso rudimentale, ma è ancora necessario esaminare il modo in cui è possibile determinare se un utente è autenticato o anonimo. In alcuni scenari è possibile che si desideri visualizzare dati o informazioni differenti a seconda che un utente autenticato o anonimo stia visitando la pagina. Inoltre, spesso è necessario identificare l'identità dell'utente autenticato.

Viene ora aumentata la pagina default. aspx esistente per illustrare queste tecniche. In default. aspx aggiungere due controlli Panel, uno denominato AuthenticatedMessagePanel e un altro denominato AnonymousMessagePanel. Aggiungere un controllo etichetta denominato WelcomeBackMessage nel primo pannello. Nel secondo pannello aggiungere un controllo collegamento ipertestuale, impostare la relativa proprietà Text su "log in" e la relativa proprietà NavigateUrl su "~/login.aspx" ". A questo punto il markup dichiarativo per default. aspx dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Come si può intuire ora, l'idea è quella di visualizzare solo i AuthenticatedMessagePanel ai visitatori autenticati e solo il AnonymousMessagePanel a visitatori anonimi. A tale scopo, è necessario impostare le proprietà visibili di questi pannelli a seconda che l'utente sia connesso o meno.

La [Proprietà Request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) restituisce un valore booleano che indica se la richiesta è stata autenticata. Immettere il codice seguente nella pagina\_caricare il codice del gestore eventi:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Con questo codice, visitare default. aspx tramite un browser. Supponendo che sia ancora necessario effettuare l'accesso, verrà visualizzato un collegamento alla pagina di accesso (vedere la figura 11). Fare clic su questo collegamento e accedere al sito. Come illustrato nel passaggio 3, dopo aver immesso le credenziali si verrà restituiti a default. aspx, ma questa volta la pagina visualizzerà "Welcome back!". messaggio (vedere la figura 12).

![Quando si visita in modo anonimo, viene visualizzato un collegamento di accesso](an-overview-of-forms-authentication-cs/_static/image27.png)

**Figura 11**: quando si visita in modo anonimo, viene visualizzato un collegamento di accesso

![Agli utenti autenticati viene visualizzato il](an-overview-of-forms-authentication-cs/_static/image28.png)

**Figura 12**: gli utenti autenticati vengono visualizzati come "Bentornato!" Messaggio

È possibile determinare l'identità dell'utente attualmente connesso tramite la [proprietà utente](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)dell' [oggetto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). L'oggetto HttpContext rappresenta le informazioni sulla richiesta corrente e rappresenta la casa per gli oggetti ASP.NET comuni come Response, Request e Session, tra gli altri. La proprietà User rappresenta il contesto di sicurezza della richiesta HTTP corrente e implementa l' [interfaccia IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

La proprietà User è impostata da FormsAuthenticationModule. In particolare, quando FormsAuthenticationModule trova un ticket di autenticazione basata su form nella richiesta in ingresso, crea un nuovo oggetto GenericPrincipal e lo assegna alla proprietà User.

Gli oggetti Principal (ad esempio GenericPrincipal) forniscono informazioni sull'identità dell'utente e i ruoli a cui appartengono. L'interfaccia IPrincipal definisce due membri:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) : metodo che restituisce un valore booleano che indica se l'entità appartiene al ruolo specificato.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) : proprietà che restituisce un oggetto che implementa l' [interfaccia IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). L'interfaccia IIdentity definisce tre proprietà: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)e [Name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

È possibile determinare il nome del visitatore corrente usando il codice seguente:

String currentUsersName = User.Identity.Name;

Quando si usa l'autenticazione basata su form, viene creato un [oggetto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) per la proprietà Identity di GenericPrincipal. La classe FormsIdentity restituisce sempre la stringa "Forms" per la proprietà AuthenticationType e true per la relativa proprietà IsAuthenticated. La proprietà Name restituisce il nome utente specificato durante la creazione del ticket di autenticazione basata su form. Oltre a queste tre proprietà, FormsIdentity include l'accesso al ticket di autenticazione sottostante tramite la relativa [Proprietà ticket](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La proprietà ticket restituisce un oggetto di tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), che include proprietà quali la scadenza, la persistenza, l'emissione, il nome e così via.

Il punto importante da prendere in questo caso è che il parametro *username* specificato nei metodi FormsAuthentication. GetAuthCookie (*username*, *PersistCookie*), FormsAuthentication. SetAuthCookie *(username*, *persistCookie*) e FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) corrisponde al valore restituito da User.Identity.Name. Inoltre, il ticket di autenticazione creato da questi metodi è disponibile eseguendo il cast di User. Identity a un oggetto FormsIdentity e quindi accedendo alla proprietà ticket:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Viene ora fornito un messaggio più personalizzato in default. aspx. Aggiornare la pagina\_gestore dell'evento Load in modo che alla proprietà Text dell'etichetta WelcomeBackMessage venga assegnata la stringa "Welcome back, *username*!"

WelcomeBackMessage. Text = "Welcome back," + User.Identity.Name + "!";

La figura 13 Mostra l'effetto di questa modifica (quando si accede come utente Scott).

![Il messaggio di benvenuto include il nome dell'utente attualmente connesso](an-overview-of-forms-authentication-cs/_static/image29.png)

**Figura 13**: il messaggio di benvenuto include il nome dell'utente attualmente connesso

### <a name="using-the-loginview-and-loginname-controls"></a>Utilizzo dei controlli LoginView e LoginName

La visualizzazione di contenuti diversi per utenti autenticati e anonimi è un requisito comune. viene visualizzato il nome dell'utente attualmente connesso. Per questo motivo, ASP.NET include due controlli Web che forniscono la stessa funzionalità illustrata nella figura 13, ma senza la necessità di scrivere una singola riga di codice.

Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) è un controllo Web basato su modelli che semplifica la visualizzazione di dati diversi agli utenti autenticati e anonimi. Il LoginView include due modelli predefiniti:

- AnonymousTemplate: qualsiasi markup aggiunto a questo modello viene visualizzato solo per i visitatori anonimi.
- LoggedInTemplate: il markup di questo modello viene visualizzato solo per gli utenti autenticati.

Aggiungere il controllo LoginView alla pagina master del sito, site. master. Anziché aggiungere solo il controllo LoginView, tuttavia, aggiungere un nuovo controllo ContentPlaceHolder e quindi inserire il controllo LoginView all'interno di tale nuovo ContentPlaceHolder. La logica per questa decisione diventerà presto evidente.

> [!NOTE]
> Oltre a AnonymousTemplate e LoggedInTemplate, il controllo LoginView può includere modelli specifici del ruolo. I modelli specifici dei ruoli mostrano il markup solo agli utenti che appartengono a un ruolo specificato. Si esamineranno le funzionalità basate sui ruoli del controllo LoginView in un'esercitazione futura.

Per iniziare, aggiungere un ContentPlaceHolder denominato LoginContent nella pagina master all'interno dell'elemento di navigazione &lt;div&gt;. È possibile trascinare semplicemente un controllo ContentPlaceHolder dalla casella degli strumenti nella visualizzazione origine, inserendo il markup risultante immediatamente sopra il menu "TODO:..." testo.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Aggiungere quindi un controllo LoginView in LoginContent ContentPlaceHolder. Il contenuto inserito nei controlli ContentPlaceHolder della pagina master viene considerato *contenuto predefinito* per ContentPlaceHolder. Ovvero, le pagine ASP.NET che utilizzano questa pagina master possono specificare il proprio contenuto per ogni ContentPlaceHolder o utilizzare il contenuto predefinito della pagina master.

Il controllo LoginView e altri controlli correlati all'account di accesso si trovano nella scheda account di accesso della casella degli strumenti.

![Controllo LoginView nella casella degli strumenti](an-overview-of-forms-authentication-cs/_static/image30.png)

**Figura 14**: controllo LoginView nella casella degli strumenti

Successivamente, aggiungere due elementi &lt;BR/&gt; subito dopo il controllo LoginView, ma sempre all'interno di ContentPlaceHolder. A questo punto, la navigazione &lt;il markup dell'elemento div&gt; dovrebbe avere un aspetto simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

È possibile definire i modelli di LoginView dalla finestra di progettazione o dal markup dichiarativo. Dalla finestra di progettazione di Visual Studio, espandere lo smart tag di LoginView, che elenca i modelli configurati in un elenco a discesa. Digitare il testo "Hello, forestiero" in AnonymousTemplate; Aggiungere quindi un controllo collegamento ipertestuale e impostare le relative proprietà Text e NavigateUrl rispettivamente su "log in" e "~/login.aspx" ".

Dopo aver configurato il AnonymousTemplate, passare a LoggedInTemplate e immettere il testo "Bentornato". Trascinare quindi un controllo LoginName dalla casella degli strumenti in LoggedInTemplate, inserendolo immediatamente dopo il testo "Bentornato". Il [controllo LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), come implica il nome, Visualizza il nome dell'utente attualmente connesso. Internamente, il controllo LoginName restituisce semplicemente la proprietà User.Identity.Name

Dopo aver apportato queste aggiunte ai modelli di LoginView, il markup dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Con questa aggiunta alla pagina master site. master, a ogni pagina del sito Web verrà visualizzato un messaggio diverso a seconda che l'utente sia autenticato. Nella figura 15 viene illustrata la pagina default. aspx quando viene visitata tramite un browser dall'utente Jisun. Il messaggio "Welcome back, Jisun" viene ripetuto due volte: una volta nella sezione di navigazione della pagina master a sinistra (tramite il controllo LoginView appena aggiunto) e una volta nell'area del contenuto default. aspx (tramite i controlli Panel e la logica a livello di codice).

![Il controllo LoginView Visualizza](an-overview-of-forms-authentication-cs/_static/image31.png)

**Figura 15**: il controllo LoginView Visualizza "Welcome back, Jisun".

Poiché l'oggetto LoginView è stato aggiunto alla pagina master, può essere visualizzato in ogni pagina del sito. Tuttavia, potrebbero essere presenti pagine Web in cui non si vuole visualizzare questo messaggio. Una pagina di questo tipo è la pagina di accesso, dal momento che un collegamento alla pagina di accesso sembra fuori posto. Poiché il controllo LoginView è stato inserito in un ContentPlaceHolder nella pagina master, è possibile eseguire l'override di questo markup predefinito nella pagina di contenuto. Aprire login. aspx e passare alla finestra di progettazione. Poiché non è stato definito in modo esplicito un controllo contenuto in login. aspx per LoginContent ContentPlaceHolder nella pagina master, nella pagina di accesso verrà visualizzato il markup predefinito della pagina master per questo ContentPlaceHolder. È possibile visualizzarlo tramite la finestra di progettazione: LoginContent ContentPlaceHolder Visualizza il markup predefinito (il controllo LoginView).

[![la pagina di accesso Mostra il contenuto predefinito per LoginContent ContentPlaceHolder della pagina master](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Figura 16**: la pagina di accesso Mostra il contenuto predefinito per il LoginContent ContentPlaceHolder della pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image34.png))

Per eseguire l'override del markup predefinito per LoginContent ContentPlaceHolder, è sufficiente fare clic con il pulsante destro del mouse sull'area nella finestra di progettazione e scegliere l'opzione Crea contenuto personalizzato dal menu di scelta rapida. Quando si usa Visual Studio 2008, ContentPlaceHolder include uno smart tag che, se selezionato, offre la stessa opzione. In questo modo viene aggiunto un nuovo controllo contenuto al markup della pagina, che consente di definire contenuto personalizzato per questa pagina. È possibile aggiungere qui un messaggio personalizzato, ad esempio "Accedi...", ma lasciarlo vuoto.

> [!NOTE]
> In Visual Studio 2005 la creazione di contenuto personalizzato consente di creare un controllo contenuto vuoto nella pagina ASP.NET. In Visual Studio 2008, tuttavia, la creazione di contenuto personalizzato copia il contenuto predefinito della pagina master nel controllo contenuto appena creato. Se si usa Visual Studio 2008, dopo aver creato il nuovo controllo contenuto assicurarsi di cancellare il contenuto copiato dalla pagina master.

Nella figura 17 viene illustrata la pagina login. aspx quando viene visitata da un browser dopo aver apportato questa modifica. Si noti che nel&gt; &lt;di spostamento a sinistra non è presente alcun messaggio "Hello, forestiero" o *"Bentornato"* .

[![la pagina di accesso nasconde il markup predefinito di LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Figura 17**: la pagina di accesso nasconde il markup predefinito di LoginContent ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image37.png))

## <a name="step-5-logging-out"></a>Passaggio 5: disconnessione

Nel passaggio 3 è stato illustrato come creare una pagina di accesso per l'accesso di un utente al sito, ma è ancora necessario vedere come disconnettere un utente. Oltre ai metodi per la registrazione di un utente in, la classe FormsAuthentication fornisce anche un [metodo di disconnessione](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Il metodo di disconnessione Elimina semplicemente il ticket di autenticazione basata su form, quindi scollega l'utente dal sito.

L'offerta di un collegamento di disconnessione è una funzionalità comune che ASP.NET include un controllo appositamente progettato per disconnettere un utente. Il [controllo LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Visualizza un LinkButton "login" o un LinkButton di disconnessione, a seconda dello stato di autenticazione dell'utente. Viene eseguito il rendering di un LinkButton "login" per gli utenti anonimi, mentre un LinkButton di "disconnessione" viene visualizzato agli utenti autenticati. Il testo per gli LinkButton "login" e "logout" può essere configurato tramite le proprietà LoginText e LogoutText di LoginStatus.

Se si fa clic sull'oggetto LinkButton "login", viene eseguito un postback dal quale viene eseguito un reindirizzamento alla pagina di accesso. Se si fa clic sul controllo LinkButton "logout", il controllo LoginStatus richiama il metodo FormsAuthentication. confirmato e quindi reindirizza l'utente a una pagina. La pagina a cui viene reindirizzato l'utente disconnesso dipende dalla proprietà LogoutAction, che può essere assegnata a uno dei tre valori seguenti:

- Refresh: valore predefinito. reindirizza l'utente alla pagina che stava semplicemente visitando. Se la pagina appena visitata non consente utenti anonimi, il FormsAuthenticationModule reindirizza automaticamente l'utente alla pagina di accesso.

È possibile che si voglia sapere perché viene eseguito un reindirizzamento qui. Se l'utente desidera rimanere nella stessa pagina, perché la necessità del reindirizzamento esplicito? Il motivo è che, quando si fa clic sul LinkButton di disconnessione, l'utente ha ancora il ticket di autenticazione basata su form nella raccolta di cookie. Di conseguenza, la richiesta di postback è una richiesta autenticata. Il controllo LoginStatus chiama il metodo di disconnessione, ma ciò si verifica dopo che FormsAuthenticationModule ha autenticato l'utente. Pertanto, un reindirizzamento esplicito induce il browser a richiedere nuovamente la pagina. Quando il browser richiede nuovamente la pagina, il ticket di autenticazione basata su form è stato rimosso e pertanto la richiesta in ingresso è anonima.

- Reindirizzamento: l'utente viene reindirizzato all'URL specificato dalla proprietà LogoutPageUrl di LoginStatus.
- RedirectToLoginPage: l'utente viene reindirizzato alla pagina di accesso.

Aggiungere un controllo LoginStatus alla pagina master e configurarlo in modo da usare l'opzione di reindirizzamento per inviare l'utente a una pagina che visualizza un messaggio che conferma che è stato disconnesso. Per iniziare, creare una pagina nella directory radice denominata logout. aspx. Non dimenticare di associare questa pagina alla pagina master site. master. Immettere quindi un messaggio nel markup della pagina che spieghi all'utente che è stato disconnesso.

Tornare quindi alla pagina master del sito. master e aggiungere un controllo LoginStatus sotto l'oggetto LoginView in LoginContent ContentPlaceHolder. Impostare la proprietà LogoutAction del controllo LoginStatus su Redirect e la relativa proprietà LogoutPageUrl su "~/logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Poiché LoginStatus è all'esterno del controllo LoginView, viene visualizzato per gli utenti anonimi e autenticati, ma è accettabile perché LoginStatus visualizzerà correttamente un LinkButton "login" o "logout". Con l'aggiunta del controllo LoginStatus, il collegamento ipertestuale "Accedi" in AnonymousTemplate è superfluo, quindi rimuoverlo.

La figura 18 Mostra default. aspx quando Jisun visita. Si noti che nella colonna sinistra viene visualizzato il messaggio "Welcome back, Jisun" insieme a un collegamento per la disconnessione. Se si fa clic sull'oggetto LinkButton di disconnessione, viene effettuato un postback, viene firmato Jisun dal sistema e quindi viene reindirizzato a logout. aspx. Come illustrato nella figura 19, quando Jisun raggiunge logout. aspx, è già stato disconnesso ed è quindi anonimo. Di conseguenza, nella colonna sinistra viene visualizzato il testo "Welcome, Forestiere" e un collegamento alla pagina di accesso.

[![viene visualizzato default. aspx](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Figura 18**: default. aspx Mostra "Welcome back, Jisun" insieme a un LinkButton "logout" ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image40.png))

[Viene visualizzato ![logout. aspx](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Figura 19**: logout. aspx Mostra "Welcome, Forestiere" insieme a un LinkButton "login" ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-forms-authentication-cs/_static/image43.png))

> [!NOTE]
> Si consiglia di personalizzare la pagina Logout. aspx per nascondere il LoginContent ContentPlaceHolder della pagina master, come per login. aspx nel passaggio 4. Il motivo è che il controllo LinkButton "login" di cui è stato eseguito il rendering dal controllo LoginStatus (quello sotto "Hello, forestiero") Invia l'utente alla pagina di accesso passando l'URL corrente nel parametro QueryString ReturnUrl. In breve, se un utente che ha effettuato la disconnessione fa clic su questo LinkButton "login" di LoginStatus e quindi esegue l'accesso, viene reindirizzato di nuovo a logout. aspx, che potrebbe facilmente confondere l'utente.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata avviata un'analisi del flusso di lavoro di autenticazione basata su form e quindi è stata attivata l'implementazione dell'autenticazione basata su form in un'applicazione ASP.NET. L'autenticazione basata su form è basata su FormsAuthenticationModule, che ha due responsabilità: identificare gli utenti in base al ticket di autenticazione basata su form e reindirizzare utenti non autorizzati alla pagina di accesso.

La classe FormsAuthentication del .NET Framework include metodi per la creazione, l'ispezione e la rimozione dei ticket di autenticazione basata su form. La proprietà Request. IsAuthenticated e l'oggetto utente forniscono supporto programmatico aggiuntivo per determinare se una richiesta è autenticata e le informazioni sull'identità dell'utente. Sono disponibili anche i controlli Web LoginView, LoginStatus e LoginName, che offrono agli sviluppatori un modo rapido e senza codice per eseguire molte attività comuni relative agli account di accesso. Questi e altri controlli Web correlati all'accesso vengono esaminati più dettagliatamente nelle esercitazioni future.

In questa esercitazione è stata fornita una panoramica superficiale dell'autenticazione basata su form. Non sono state esaminate le diverse opzioni di configurazione, si osservi il funzionamento dei ticket di autenticazione basata su form senza cookie oppure esplorare il modo in cui ASP.NET protegge il contenuto del ticket di autenticazione basata su form. Verranno illustrati questi argomenti e altro ancora nell' [esercitazione successiva](forms-authentication-configuration-and-advanced-topics-cs.md).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Modifiche tra IIS6 e IIS7 Security](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controlli ASP.NET di accesso](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Sicurezza, appartenenza e gestione dei ruoli Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Elemento `<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Elemento `<forms>` per `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione video sugli argomenti contenuti in questa esercitazione

- [Uso dell'autenticazione basata su form di base in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è stato esaminato da molti revisori utili. I revisori del lead per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](security-basics-and-asp-net-support-cs.md)
> [Successivo](forms-authentication-configuration-and-advanced-topics-cs.md)
