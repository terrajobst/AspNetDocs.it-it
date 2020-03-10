---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET and Web Tools 2013,2 per le note sulla versione Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623193"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Note sulla versione di ASP.NET and Web Tools 2013.2 per Visual Studio 2013

[Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Note di installazione

ASP.NET and Web Tools per Visual Studio 2013,2 sono aggregate nel programma di installazione principale e possono essere scaricate come parte di [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentazione

Le esercitazioni e altre informazioni su ASP.NET and Web Tools per Visual Studio 2013,2 sono disponibili dal [sito Web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisiti software

ASP.NET and Web Tools per Visual Studio 2013,2 richiede Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013,2

Le sezioni seguenti descrivono le funzionalità introdotte nella versione di.

- [Un modello di progetto ASP.NET](#oneaspnet)
- [Supporto di SSL quando si avviano applicazioni Web in IIS Express](#ssl)
- [Miglioramenti dell'editor Web di Visual Studio](#vswebeditor)
- [Browser Link](#browserlink)
- [Supporto per le app Web del servizio app Azure in Visual Studio](#waws)
- [Creare risorse di Azure remote quando si crea un nuovo progetto Web](#AzureResources)
- [Miglioramenti alla pubblicazione sul Web](#webpublish)
- [Impalcatura ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Form ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [API Web ASP.NET 2.1.2](#webapi)
- [Pagine Web ASP.NET 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Componenti di Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Un modello di progetto ASP.NET

- Aggiornamenti ai modelli di progetto ASP.NET per supportare la conferma dell'account e la reimpostazione della password.
- Aggiornare API Web ASP.NET modello per supportare l'autenticazione tramite account aziendali locali.
- Il modello di ASP.NET SPA ora contiene l'autenticazione basata sulle visualizzazioni MVC e lato server. Il modello dispone di un controller WebAPI a cui è possibile accedere solo da utenti autenticati.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Supporto di SSL quando si avviano applicazioni Web in IIS Express

Per eliminare l'avviso di sicurezza durante l'esplorazione e il debug di HTTPS in localhost, è stata aggiunta una finestra di dialogo per consentire a Internet Explorer e Chrome di considerare attendibile il certificato SSL autofirmato di IIS Express.

È ad esempio possibile impostare una proprietà di un progetto Web per l'utilizzo di SSL. Fare clic su F4 per visualizzare la finestra di dialogo delle proprietà. Impostare **SSL abilitato** su true. Copiare l'URL SSL.

![Proprietà abilitata per SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Impostare la scheda Web della pagina delle proprietà del progetto Web in modo che utilizzi l'URL basato su HTTPS (l'URL SSL verrà `https://localhost:44300/` a meno che non siano stati creati in precedenza siti Web SSL).

![Imposta URL progetto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Premere CTRL+F5 per eseguire l'applicazione. Seguire le istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express.

![Avviso SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leggere la finestra di dialogo **avviso di sicurezza** e quindi fare clic su **Sì** se si vuole installare il certificato che rappresenta localhost.

![Avviso di sicurezza](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Il sito verrà visualizzato in IE o Chrome senza l'avviso del certificato nel browser.

![Pagina HTTPS senza avvisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox usa il proprio archivio certificati, quindi verrà visualizzato un avviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti dell'editor Web di Visual Studio

- **Nuovo elemento di progetto JSON ed editor**: è stato aggiunto un elemento di progetto JSON e un editor a Visual Studio. Le funzionalità dell'editor JSON correnti includono la colorazione, la convalida della sintassi, il completamento delle parentesi graffe, la struttura, l'impostazione delle opzioni degli strumenti e altro ancora.

    ![Editor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense supporta ora lo [schema JSON](http://json-schema.org/) V3 e V4. È disponibile una casella combinata dello schema per scegliere gli schemi esistenti, modificare il percorso dello schema locale o semplicemente trascinare un file JSON di progetto per ottenere il percorso relativo.

    ![IntelliSense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor schemi JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuovo editor Sass (scss)** : è stato aggiunto less in VS2013 RTM ed è ora disponibile un elemento di progetto Sass e un editor. Le funzionalità dell'editor Sass sono paragonabili a quelle dell'editor LESS e includono la colorazione, la variabile e mixin IntelliSense, i commenti e i commenti, le informazioni rapide, la formattazione, la convalida della sintassi, la struttura, la definizione goto, la selezione colori, l'impostazione dell'opzione strumenti e così via.

    ![Aggiungi nuovo elemento: foglio di stile SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor del foglio di stile](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuova selezione URL nei documenti HTML, Razor, CSS, less e Sass:** VS 2013 fornito senza selezione URL all'esterno delle pagine Web Form. La nuova selezione URL per gli editor HTML, Razor, CSS, LESS e Sass è un selettore di tipizzazione Fluent che comprende ".." e filtra gli elenchi di file in modo appropriato per i tag img e i collegamenti.

    ![Selezione URL per il tag image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selezione URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selezione URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aggiornamenti per LESS editor con l'aggiunta di altre funzionalità**
- **Aggiornamento di IntelliSense Knockout**: è stata aggiunta una sintassi Knockout non standard per Visual Studio IntelliSense, la sintassi "Ko-vs-editor ViewModel:". Può essere usato per eseguire il binding a più modelli di visualizzazione in una pagina usando i commenti nel formato:

    ![IntelliSense Knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    È stato inoltre aggiunto il supporto per la funzionalità IntelliSense nidificata, quindi è possibile esaminare gli oggetti annidati in modo approfondito nell'elemento ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelliSense visualizzato è il IntelliSense completo dell'oggetto JavaScript.

    ![IntelliSense che Visualizza l'oggetto JavaScript completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuova selezione URL nei documenti HTML, Razor, CSS, less e Sass**: vs 2013 fornito senza selezione URL all'esterno delle pagine Web Form. La nuova selezione URL per gli editor HTML, Razor, CSS, LESS e Sass è un selettore di tipizzazione Fluent che comprende ".." e filtra gli elenchi di file in modo appropriato per i tag img e i collegamenti.

    ![Selezione URL per il tag image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selezione URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selezione URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Browser Link

- Browser Link supporta ora le connessioni HTTPS e ne elenca l'elenco nel dashboard con altre connessioni purché il certificato sia considerato attendibile dal browser.
- Mapping di origine HTML statico
- Supporto di SPA per il mapping dei dati
- Aggiornamento automatico dei dati di mapping

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Supporto per le app Web del servizio app Azure in Visual Studio

- **Supporto per l'accesso ad Azure.**
- **Debug remoto e visualizzazione remota per le app Web**: è ora supportato il [debug remoto per le app web nel servizio app Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e la visualizzazione remota dei file di contenuto dell'app Web in Esplora server.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Creare risorse di Azure remote quando si crea un nuovo progetto Web

È stata aggiunta una casella di [controllo "crea risorse remote"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) di Azure nella finestra di dialogo nuova applicazione Web. Scegliendo questa opzione, sarà possibile integrare l'esperienza di creazione di una nuova applicazione Web, la configurazione del sito di pubblicazione di Azure per il test e la creazione di un profilo di pubblicazione in pochi semplici passaggi.

![Nuovo progetto con risorse di Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Pubblicazione in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Miglioramenti alla pubblicazione sul Web

- Migliorare l'esperienza utente per la pubblicazione.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Impalcatura ASP.NET

- **Supporto enum:** Se il modello USA enum, l'impalcatura MVC genererà l'elenco a discesa per enum. USA gli helper enum in MVC.
- **Supporto bootstrap**: aggiornamento dei modelli EditorFor nell'impalcatura MVC in modo che usino le classi bootstrap.
- **Supporto dei pacchetti**: i componenti di MVC e le pagine Web API aggiungono pacchetti 5,1 per MVC e API Web

Gli screenshot seguenti illustrano i modelli di ponteggi.

- Codice modello:

     ![Codice modello](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilare il codice del modello, fare clic con il pulsante destro del mouse e scegliere **Aggiungi**, **nuovo elemento con impalcatura**.

     ![Aggiungi nuovo elemento con impalcatura](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Scegliere il **controller MVC5 con le visualizzazioni, usando Entity Framework**:

     ![Aggiungi nuovo controller MVC5 con visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Aggiungere il controller** usando il modello:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Controllare il codice generato, ad esempio views/WeekdayModels/Edit. cshtml contiene `@Html.EnumDropDownListFor`: ![vista contenente EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Eseguire la pagina per visualizzare la ComboBox enum generata. si noti che se un valore può essere null, è possibile scegliere una stringa vuota per la casella combinata. Ad esempio, la pagina **Crea** Mostra quanto segue:

    ![Casella combinata che consente una stringa vuota](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM verrà rilasciato ad aprile 2014. Ecco i punti salienti delle note sulla versione, ma per altre informazioni su queste modifiche, vedere le [Note sulla versione complete](http://docs.nuget.org/docs/release-notes/nuget-2.8) .

- **Applicazioni di destinazione Windows Phone 8,1**: NuGet 2.8.1 supporta ora la destinazione Windows Phone 8,1 applicazioni che usano i moniker del Framework di destinazione ' WindowsPhoneApp ',' WPA ',' WindowsPhoneApp81' è WPA81'.

- **Risoluzione delle patch per le dipendenze**: quando si risolvono le dipendenze dei pacchetti, NuGet ha implementato in passato una strategia di selezione della versione più bassa del pacchetto principale e secondaria che soddisfa le dipendenze del pacchetto. A differenza della versione principale e secondaria, tuttavia, la versione della patch è stata sempre risolta con la versione più recente. Anche se il comportamento era ben intenzionale, ha creato un'assenza di determinismo per l'installazione dei pacchetti con dipendenze.
- **Opzione DependencyVersion**: sebbene NuGet 2,8 modifichi il comportamento *predefinito* per la risoluzione delle dipendenze, aggiunge anche un controllo più preciso sul processo di risoluzione delle dipendenze tramite l'opzione-DependencyVersion nella console di gestione pacchetti. Il commutatore consente la risoluzione delle dipendenze con la versione più bassa possibile (comportamento predefinito), la versione più alta possibile o la versione minore o patch più recente. Questa opzione funziona solo per Install-Package nel comando di PowerShell.
- **Attributo DependencyVersion**: oltre all'opzione-DependencyVersion descritta in precedenza, NuGet ha anche la possibilità di impostare un nuovo attributo nel file NuGet. config che definisce il valore predefinito, se l'opzione-DependencyVersion non è specificata in una chiamata di Install-Package. Questo valore verrà rispettato anche dalla finestra di dialogo Gestione pacchetti NuGet per tutte le operazioni di installazione dei pacchetti. Per impostare questo valore, aggiungere l'attributo seguente al file NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Anteprima delle operazioni NuGet con-whatif**: alcuni pacchetti NuGet possono avere grafici con dipendenze complete e, di conseguenza, possono essere utili durante un'operazione di installazione, disinstallazione o aggiornamento per vedere prima cosa accadrà. NuGet 2,8 aggiunge l'opzione PowerShell standard-What If ai comandi install-package, Uninstall-Package e Update-Package per consentire la visualizzazione dell'intera chiusura dei pacchetti a cui verrà applicato il comando.
- **Downgrade del pacchetto**: non è insolito installare una versione provvisoria di un pacchetto per esaminare le nuove funzionalità e quindi decidere di eseguire il rollback all'ultima versione stabile. Prima di NuGet 2,8, si trattava di un processo costituito da più passaggi per la disinstallazione del pacchetto di versioni non definitive e delle relative dipendenze, quindi l'installazione della versione precedente. Con NuGet 2,8, tuttavia, il pacchetto di aggiornamento eseguirà il rollback dell'intera chiusura del pacchetto, ad esempio l'albero delle dipendenze del pacchetto, alla versione precedente.
- **Dipendenze di sviluppo**: molti tipi diversi di funzionalità possono essere forniti come pacchetti NuGet, inclusi gli strumenti usati per ottimizzare il processo di sviluppo. Questi componenti, sebbene possano essere strumentali nello sviluppo di un nuovo pacchetto, non devono essere considerati una dipendenza del nuovo pacchetto quando viene pubblicato in seguito. NuGet 2,8 consente l'identificazione di un pacchetto nel file con estensione NuSpec come developmentDependency. Quando è installato, questi metadati verranno aggiunti anche al file Packages. config del progetto in cui è stato installato il pacchetto. Quando il file Packages. config viene analizzato in un secondo momento per le dipendenze NuGet durante NuGet. exe Pack, le dipendenze contrassegnate come dipendenze di sviluppo verranno escluse.
- **Singoli file di Packages. config per piattaforme diverse**: quando si sviluppano applicazioni per più piattaforme di destinazione, è normale che i file di progetto siano diversi per ognuno dei rispettivi ambienti di compilazione. È anche comune usare pacchetti NuGet diversi in file di progetto diversi, poiché i pacchetti hanno livelli di supporto diversi per le diverse piattaforme. NuGet 2,8 fornisce un supporto migliorato per questo scenario creando diversi file di Packages. config per diversi file di progetto specifici della piattaforma.
- **Fallback alla cache locale**: anche se i pacchetti NuGet vengono in genere usati da una raccolta remota, ad esempio la [raccolta NuGet](http://www.nuget.org) che usa una connessione di rete, esistono molti scenari in cui il client non è connesso. Senza una connessione di rete, il client NuGet non è stato in grado di installare correttamente i pacchetti, anche quando tali pacchetti erano già presenti nel computer del client nella cache NuGet locale. NuGet 2,8 aggiunge il fallback della cache automatica alla console di gestione pacchetti.

    La funzionalità di fallback della cache non richiede argomenti di comando specifici. Inoltre, il fallback della cache attualmente funziona solo nella console di gestione pacchetti. il comportamento attualmente non funziona nella finestra di dialogo Gestione pacchetti.
- **Correzioni di bug**: una delle principali correzioni di bug apportate è stata il miglioramento delle prestazioni nel comando Update-Package-REINSTALL.

    Oltre a queste funzionalità e alla precedente correzione delle prestazioni, questa versione di NuGet include anche molte altre correzioni di bug. Nella versione sono stati rilevati 181 problemi totali. Per un elenco completo degli elementi di lavoro corretti in NuGet 2,8, vedere la pagina relativa al [rilevamento dei problemi di NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) per questa versione.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Form ASP.NET

- I modelli Web Form ora illustrano come eseguire la conferma dell'account e la reimpostazione della password per ASP.NET Identity.
- Il controllo origine dati dell'entità e il provider di Dynamic Data per Entity Framework 6. Per altri dettagli, vedere il Blog MSDN seguente: [provider Dynamic Data e controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Miglioramenti del routing degli attributi](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Supporto bootstrap per i modelli di editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Supporto enum nelle viste](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Supporto intrusivo per gli attributi MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Supporto del contesto ' This ' nell'Ajax non intrusivo](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>API Web ASP.NET 2.1.2

- [Gestione degli errori globale](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Miglioramenti del routing degli attributi](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Miglioramenti della pagina della Guida](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Supporto di IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formattatore del tipo di supporto BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Supporto migliore per i filtri asincroni](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analisi delle query per la libreria di formattazione client](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Pagine Web ASP.NET 3.1.2

- Varie [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

Entity Framework è stato aggiornato alla versione 6,1 per il runtime e gli strumenti. Entity Framework (EF) 6,1 è un aggiornamento secondario Entity Framework 6 e include una serie di correzioni di bug e nuove funzionalità. Per informazioni dettagliate su EF 6.1, inclusi i collegamenti alla documentazione per le nuove funzionalità, vedere [cronologia delle versioni di Entity Framework](https://msdn.microsoft.com/data/jj574253). Le nuove funzionalità di questa versione includono:

- Il **consolidamento degli strumenti** offre un modo coerente per creare un nuovo modello EF. Questa funzionalità estende la procedura guidata ADO.NET Entity Data Model per supportare la creazione di modelli Code First, incluso reverse engineering da un database esistente. Queste funzionalità in precedenza erano disponibili in qualità beta in EF Power Tools.
- La **gestione degli errori di commit delle transazioni** fornisce la nuova funzionalità [System. Data. Entity. Infrastructure. CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) che consente di utilizzare la nuova funzionalità introdotta per intercettare le operazioni di transazione. Il **CommitFailureHandler** consente il ripristino automatico dagli errori di connessione durante il commit di una transazione.
- **IndexAttribute** consente di specificare gli indici inserendo un attributo in una proprietà (o proprietà) nel modello di Code First. Code First creerà quindi un indice corrispondente nel database.
- **L'API di mapping pubblico** fornisce l'accesso alle informazioni EF sul modo in cui le proprietà e i tipi vengono mappati alle colonne e alle tabelle nel database. Nelle versioni precedenti questa API era interna.
- **Possibilità di configurare gli intercettori tramite il file app/Web. config**, consentendo l'aggiunta degli intercettori senza ricompilare l'applicazione.
- **DatabaseLogger** è un nuovo intercettore che semplifica la registrazione di tutte le operazioni di database in un file. In combinazione con la funzionalità precedente, in questo modo è possibile attivare facilmente la registrazione delle operazioni di database per un'applicazione distribuita, senza dover ricompilare.
- Il **rilevamento delle modifiche del modello delle migrazioni** è stato migliorato in modo che le migrazioni con impalcature siano più accurate. anche le prestazioni del processo di rilevamento delle modifiche sono state migliorate.
- **Miglioramenti delle prestazioni** , tra cui operazioni di database ridotte durante l'inizializzazione, ottimizzazioni per il confronto di uguaglianza di valori null nelle query LINQ, generazione di visualizzazioni più veloce (creazione di modelli) in più scenari e materializzazione più efficiente delle entità registrate con più associazioni.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Autenticazione a due fattori**: ASP.NET Identity supporta ora l'autenticazione a due fattori. L'autenticazione a due fattori fornisce un livello di sicurezza aggiuntivo per gli account utente nel caso in cui la password venga compromessa. È inoltre disponibile la protezione per gli attacchi di forza bruta contro i codici a due fattori.
- **Blocco account:** Consente di bloccare l'utente se l'utente immette la password o i codici a due fattori in modo errato. È possibile configurare il numero di tentativi non validi e l'intervallo di tempo per gli utenti bloccati. Uno sviluppatore può facoltativamente disattivare il blocco degli account per determinati account utente, se necessario.
- **Conferma dell'account:** Il sistema ASP.NET Identity supporta ora la conferma dell'account. Si tratta di uno scenario abbastanza comune nella maggior parte dei siti Web di oggi, in cui, quando si esegue la registrazione per un nuovo account nel sito Web, è necessario confermare la posta elettronica prima di poter eseguire qualsiasi operazione nel sito Web. La conferma tramite posta elettronica è utile perché impedisce la creazione di account fasulli. Questa operazione è estremamente utile se si usa la posta elettronica come metodo di comunicazione con gli utenti del sito Web, ad esempio siti di forum, servizi bancari, ecommerce o siti Web di social networking.
- **Reimpostazione della password:** La reimpostazione della password è una funzionalità in cui l'utente può reimpostare le password se ha dimenticato la password.
- **Indicatore di sicurezza (disconnettersi ovunque):** Supporta un modo per rigenerare il token di sicurezza per l'utente nei casi in cui l'utente modifica la password o altre informazioni correlate alla sicurezza, ad esempio la rimozione di un account di accesso associato, ad esempio Facebook, Google, account Microsoft e così via. Questa operazione è necessaria per garantire che tutti i token generati con la vecchia password vengano invalidati. Nel progetto di esempio, se si modifica la password dell'utente, viene generato un nuovo token per l'utente e tutti i token precedenti vengono invalidati. Questa funzionalità offre un livello aggiuntivo di sicurezza per l'applicazione, poiché quando si cambia la password, si verrà disconnessi da qualsiasi luogo (tutti gli altri browser) in cui è stato eseguito l'accesso a questa applicazione.
- **Rendere estendibile il tipo di chiave primaria per utenti e ruoli**: In ASP.NET Identity 1,0, il tipo di chiave primaria per gli utenti e i ruoli della tabella è costituito da stringhe. Ciò significa che quando il sistema di ASP.NET Identity è stato salvato in modo permanente in SQL Server utilizzando Entity Framework, era in uso nvarchar. Sono state rilevate molte discussioni su questa implementazione predefinita su Stack Overflow e sulla base del feedback in arrivo. È stato fornito un hook di estensibilità in cui è possibile specificare la chiave primaria della tabella users and Roles. Questo hook di estendibilità è particolarmente utile se si esegue la migrazione dell'applicazione e l'applicazione archivia gli UserId sono GUID o int.
- **Supporto di IQueryable su utenti e ruoli**: è stato aggiunto il supporto per IQueryable in UsersStore e RolesStore, è possibile ottenere facilmente l'elenco di utenti e ruoli.
- **Supporto dell'operazione di eliminazione tramite UserManager**
- **Indicizzazione sul nome utente**: nell'implementazione di ASP.NET Identity Entity Framework, è stato aggiunto un indice univoco al nome utente usando il nuovo INDEXATTRIBUTE in Ef 6.1.0. In questo modo si garantisce che i nomi utente siano sempre univoci e che non esistano race condition in cui è possibile che si verifichino nomi utente duplicati.
- **Validator password migliorata:** Il validator della password fornito in ASP.NET Identity 1,0 è un validator della password di base che convalida solo la lunghezza minima. È disponibile un nuovo validator della password che offre un maggiore controllo sulla complessità della password. Si noti che anche se si attivano tutte le impostazioni della password, si consiglia di abilitare l'autenticazione a due fattori per gli account utente.
- **Middleware IdentityFactory/CreatePerOwinContext**:

    - **User Manager**: è possibile usare l'implementazione Factory per ottenere un'istanza di usermanager dal contesto OWIN. Questo modello è simile a quello usato per ottenere AuthenticationManager dal contesto di OWIN per l'accesso e la disconnessione. Si tratta di un metodo consigliato per ottenere un'istanza di UserManager per ogni richiesta per l'applicazione.
    - **DbContextFactory**: ASP.NET Identity utilizza Entity Framework per il salvataggio permanente del sistema di identità in SQL Server. A tale scopo, il sistema di identità ha un riferimento a ApplicationDbContext. Il middleware DbContextFactory restituisce un'istanza di ApplicationDbContext per ogni richiesta che è possibile usare nell'applicazione.
- **Pacchetto NuGet**di esempi ASP.NET Identity: il pacchetto NuGet di esempio può semplificare l'installazione e l'esecuzione di esempi per ASP.NET Identity e seguire le procedure consigliate. Si tratta di un'applicazione MVC ASP.NET di esempio. Modificare il codice in base all'applicazione prima di distribuirlo nell'ambiente di produzione. L'esempio deve essere installato in un'applicazione ASP.NET vuota. Per ulteriori informazioni sul pacchetto, visitare il post di Blog seguente: [annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

In questa versione sono stati risolti numerosi bug. Per informazioni più dettagliate, vedere le [Note sulla versione per la versione 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) .

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

In questa versione sono stati risolti numerosi bug. Per informazioni più dettagliate, vedere le [Note sulla versione per la versione 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) .
