---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di test-5 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635128"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione in IIS come ambiente di test-5 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica

Questa esercitazione illustra come distribuire un'applicazione Web ASP.NET in IIS nel computer locale.

Quando si sviluppa un'applicazione, in genere si esegue il test eseguendola in Visual Studio. Per impostazione predefinita, questo significa che si sta usando il Server di sviluppo Visual Studio (noto anche come Cassini). Il Server di sviluppo Visual Studio consente di eseguire facilmente test durante lo sviluppo in Visual Studio, ma non funziona esattamente come IIS. Di conseguenza, è possibile che un'applicazione venga eseguita correttamente quando si esegue il test in Visual Studio, ma non riesce quando viene distribuita in IIS in un ambiente host.

È possibile testare l'applicazione in modo più affidabile nei modi seguenti:

1. Usare IIS Express o IIS completo invece del Server di sviluppo Visual Studio quando si esegue il test in Visual Studio durante lo sviluppo. Questo metodo in genere emula più accuratamente il modo in cui il sito verrà eseguito in IIS. Tuttavia, questo metodo non verifica il processo di distribuzione o verifica che il risultato del processo di distribuzione venga eseguito correttamente.
2. Distribuire l'applicazione in IIS nel computer di sviluppo usando lo stesso processo che verrà usato in un secondo momento per distribuirlo nell'ambiente di produzione. Questo metodo consente di convalidare il processo di distribuzione, oltre a convalidare che l'applicazione verrà eseguita correttamente in IIS.
3. Distribuire l'applicazione in un ambiente di test più vicino possibile all'ambiente di produzione. Poiché l'ambiente di produzione per queste esercitazioni è un provider di hosting di terze parti, l'ambiente di test ideale sarebbe un secondo account con il provider di hosting. Il secondo account verrà usato solo per i test, ma verrà configurato esattamente come l'account di produzione.

Questa esercitazione illustra i passaggi per l'opzione 2. Il materiale sussidiario per l'opzione 3 è disponibile alla fine dell'esercitazione [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . al termine di questa esercitazione sono disponibili collegamenti alle risorse per l'opzione 1.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurazione dell'applicazione per l'esecuzione con attendibilità media

Prima di installare IIS e la relativa distribuzione, è necessario modificare l'impostazione di un file Web. config per fare in modo che il sito venga eseguito in modo analogo a quello in un tipico ambiente di hosting condiviso.

I provider di hosting in genere eseguono il sito Web in *attendibilità media*, il che significa che alcune operazioni non sono consentite. Ad esempio, il codice dell'applicazione non riesce ad accedere al registro di sistema di Windows e non è in grado di leggere o scrivere file esterni alla gerarchia di cartelle dell'applicazione. Per impostazione predefinita, l'applicazione viene eseguita con *attendibilità elevata* nel computer locale, il che significa che l'applicazione potrebbe essere in grado di eseguire operazioni che potrebbero avere esito negativo quando viene distribuita nell'ambiente di produzione. Pertanto, per fare in modo che l'ambiente di test rispecchi in modo più accurato l'ambiente di produzione, l'applicazione verrà configurata per l'esecuzione in attendibilità media.

Nel file Web. config dell'applicazione aggiungere un elemento **trust** nell'elemento **System. Web** , come illustrato in questo esempio.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

L'applicazione verrà ora eseguita in attendibilità media in IIS anche nel computer locale. Questa impostazione consente di rilevare il prima possibile qualsiasi tentativo da codice dell'applicazione di eseguire operazioni che potrebbero avere esito negativo in produzione.

> [!NOTE]
> Se si usa Migrazioni Code First di Entity Framework, assicurarsi che sia installata la versione 5,0 o successiva. In Entity Framework versione 4,3, le migrazioni richiedono l'attendibilità totale per aggiornare lo schema del database.

## <a name="installing-iis-and-web-deploy"></a>Installazione di IIS e Distribuzione Web

Per eseguire la distribuzione in IIS nel computer di sviluppo, è necessario che siano installati IIS e Distribuzione Web. Questi non sono inclusi nella configurazione predefinita di Windows 7. Se sono già stati installati sia IIS che Distribuzione Web, passare alla sezione successiva.

L'installazione [guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx) è il modo migliore per installare iis e distribuzione Web, poiché l'installazione guidata piattaforma Web installa una configurazione consigliata per IIS e installa automaticamente i prerequisiti per iis e distribuzione Web se necessario.

Per eseguire l'installazione guidata piattaforma Web per installare IIS e Distribuzione Web, utilizzare il collegamento seguente. Se IIS è già installato, Distribuzione Web o uno dei componenti necessari, l'installazione guidata piattaforma Web installa solo le informazioni mancanti.

- [Installare IIS e Distribuzione Web usando WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Impostazione del pool di applicazioni predefinito su .NET 4

Dopo l'installazione di IIS, eseguire **Gestione IIS** per assicurarsi che il .NET Framework versione 4 venga assegnato al pool di applicazioni predefinito.

Dal menu **Start** di Windows selezionare **Esegui**, immettere "inetmgr", quindi fare clic su **OK**. Se il comando **Run** non è presente nel menu **Start** , è possibile premere il tasto Windows e R per aprirlo. In alternativa, fare clic con il pulsante destro del mouse sulla barra delle applicazioni, scegliere **Proprietà**, selezionare la scheda **menu Start** , fare clic su **Personalizza**e selezionare **Esegui comando**.

Nel riquadro **connessioni** espandere il nodo del server e selezionare **pool di applicazioni**. Nel riquadro **pool di applicazioni** , se **DefaultAppPool** viene assegnato a .NET Framework versione 4 come illustrato nella figura seguente, passare alla sezione successiva.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se vengono visualizzati solo due pool di applicazioni e entrambi sono impostati sulla .NET Framework 2,0, è necessario installare ASP.NET 4 in IIS:

- Aprire una finestra del prompt dei comandi facendo clic con il pulsante destro del mouse su **prompt dei comandi** nel menu **Start** di Windows e selezionando **Esegui come amministratore**. Eseguire quindi [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) per installare ASP.NET 4 in IIS, usando i comandi seguenti. Nei sistemi a 64 bit sostituire "Framework" con "Framework64".

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Questo comando crea nuovi pool di applicazioni per il .NET Framework 4, ma il pool di applicazioni predefinito verrà comunque impostato su 2,0. Si distribuirà un'applicazione destinata a .NET 4 al pool di applicazioni, quindi è necessario modificare il pool di applicazioni in .NET 4.

Se è stato chiuso **Gestione IIS**, eseguirlo di nuovo, espandere il nodo del server e fare clic su **pool di applicazioni** per visualizzare di nuovo il riquadro pool di **applicazioni** .

Nel riquadro **pool di applicazioni** fare clic **su DefaultAppPool**, quindi nel riquadro **azioni** fare clic su **impostazioni di base**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Nella finestra di dialogo **Modifica pool di applicazioni** modificare **.NET Framework versione** in **.NET Framework v 4.0.30319** e fare clic su **OK**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

A questo punto è possibile procedere alla pubblicazione in IIS.

## <a name="publishing-to-iis"></a>Pubblicazione in IIS

Esistono diversi modi per eseguire la distribuzione con Visual Studio 2010 e Distribuzione Web:

- Usare la pubblicazione con un clic di Visual Studio.
- Creare un *pacchetto di distribuzione* e installarlo utilizzando l'interfaccia utente di gestione IIS. Il pacchetto di distribuzione è costituito da un file con *estensione zip* che contiene tutti i file e i metadati necessari per installare un sito in IIS.
- Creare un pacchetto di distribuzione e installarlo utilizzando la riga di comando.

Il processo passato nelle esercitazioni precedenti per configurare Visual Studio per automatizzare le attività di distribuzione si applica a tutti questi tre metodi. In queste esercitazioni verrà usato il primo di questi metodi. Per informazioni sull'uso dei pacchetti di distribuzione, vedere [mappa del contenuto della distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Prima della pubblicazione, assicurarsi di eseguire Visual Studio in modalità amministratore. Nel menu **Start** di Windows 7, fare clic con il pulsante destro del mouse sull'icona per la versione di Visual Studio in uso e scegliere **Esegui come amministratore**. La modalità amministratore è obbligatoria per la pubblicazione solo quando si esegue la pubblicazione in IIS nel computer locale.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity (non sul progetto CONTOSOUNIVERSITY. dal) e scegliere **pubblica**.

Viene visualizzata la procedura guidata **Pubblica sito Web** .

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Nell'elenco a discesa selezionare **&lt;nuovo...&gt;** .

Nella finestra di dialogo **nuovo profilo** immettere "test", quindi fare clic su **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Questo nome è lo stesso del nodo centrale del file di trasformazione Web. test. config creato in precedenza. Questo corrisponde a ciò che determina l'applicazione delle trasformazioni Web. test. config quando si esegue la pubblicazione utilizzando questo profilo.

La procedura guidata passa automaticamente alla scheda **connessione** .

Nella casella **URL servizio** immettere *localhost*.

Nella casella **sito/applicazione** immettere *Default Web site/ContosoUniversity*.

Nella casella **URL di destinazione** immettere `http://localhost/ContosoUniversity`.

L'impostazione dell' **URL di destinazione** non è obbligatoria. Al termine della distribuzione dell'applicazione, Visual Studio apre automaticamente il browser predefinito a questo URL. Se non si desidera che il browser venga aperto automaticamente dopo la distribuzione, lasciare vuota questa casella.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette ed è possibile connettersi a IIS nel computer locale.

Un segno di spunta verde verifica che la connessione abbia esito positivo.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Fare clic su **Avanti** per passare alla scheda **Impostazioni** .

Nella casella di riepilogo a discesa **configurazione** è specificata la configurazione della build da distribuire. Il valore predefinito è release, ovvero quello che si desidera.

Lasciare deselezionata la casella **di controllo Rimuovi file aggiuntivi nella destinazione** . Poiché si tratta della prima distribuzione, non saranno ancora presenti file nella cartella di destinazione.

Nella sezione **database** immettere il valore seguente nella casella stringa di connessione per **schoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Il processo di distribuzione inserisce questa stringa di connessione nel file Web. config distribuito perché è selezionata l'opzione **Usa questa stringa di connessione in fase di esecuzione** .

Inoltre, in **schoolContext**selezionare **applica migrazioni Code First**. Questa opzione fa in modo che il processo di distribuzione configuri il file Web. config distribuito per specificare l'inizializzatore `MigrateDatabaseToLatestVersion`. Questo inizializzatore aggiorna automaticamente il database alla versione più recente quando l'applicazione accede al database per la prima volta dopo la distribuzione.

Nella casella stringa di connessione per **DefaultConnection**immettere il valore seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lasciare deselezionata l' **aggiornamento database** . Il database di appartenenza verrà distribuito copiando il file SDF nei dati dell'app\_e non si vuole che il processo di distribuzione esegua altre operazioni con il database.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Fare clic su **Avanti** per passare alla scheda **Anteprima** .

Nella scheda **Anteprima** fare clic su **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Fare clic su **Pubblica**.

Se Visual Studio non è in modalità amministratore, è possibile che venga visualizzato un messaggio di errore che indica un errore di autorizzazione. In tal caso, chiudere Visual Studio, aprirlo in modalità amministratore e provare di nuovo a eseguire la pubblicazione.

Se Visual Studio è in modalità amministratore, la finestra di **output** segnala la compilazione e la pubblicazione riuscite.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Il browser si apre automaticamente alla Home page di Contoso University in esecuzione in IIS nel computer locale.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test nell'ambiente di test

Si noti che l'indicatore di ambiente Mostra "(test)" anziché "(dev)", che indica che la trasformazione *Web. config* per l'indicatore dell'ambiente è stata completata.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Eseguire la pagina **students** per verificare che il database distribuito non disponga di studenti. Quando si seleziona questa pagina, l'operazione di caricamento potrebbe richiedere alcuni minuti perché Code First crea il database e quindi esegue il `Seed` metodo. Questa operazione non è stata eseguita nel home page perché l'applicazione non ha ancora tentato di accedere al database.

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Eseguire la pagina **insegnanti** per verificare che Code First il seeding del database con i dati dell'insegnante:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selezionare **Aggiungi studenti** dal menu **students (studenti** ), aggiungere uno studente, quindi visualizzare il nuovo studente nella pagina **students (studenti** ) per verificare che sia possibile scrivere correttamente nel database:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Dal menu **corsi** selezionare **Aggiorna crediti**. La pagina di **aggiornamento crediti** richiede autorizzazioni di amministratore, pertanto viene visualizzata la pagina **di accesso** . Immettere le credenziali dell'account amministratore create in precedenza ("admin" e "pas $ w0rd"). Viene visualizzata la pagina **Update Credits** , che verifica che l'account Administrator creato nell'esercitazione precedente sia stato distribuito correttamente nell'ambiente di test.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verificare che sia presente una cartella *ELMAH* con solo il file segnaposto.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisione delle modifiche automatiche di Web. config per Migrazioni Code First

Aprire il file *Web. config* nell'applicazione distribuita in *C:\inetpub\wwwroot\ContosoUniversity* . è possibile vedere il punto in cui il processo di distribuzione è stato configurato migrazioni Code First per aggiornare automaticamente il database alla versione più recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Il processo di distribuzione ha creato anche una nuova stringa di connessione per Migrazioni Code First da usare esclusivamente per l'aggiornamento dello schema del database:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Questa stringa di connessione aggiuntiva consente di specificare un account utente per gli aggiornamenti dello schema del database e un account utente diverso per l'accesso ai dati dell'applicazione. Ad esempio, è possibile assegnare il ruolo di proprietario del\_di database a Migrazioni Code First e i ruoli DB\_DataReader e DB\_DataWriter all'applicazione. Si tratta di un modello di difesa in profondità comune che impedisce al codice potenzialmente dannoso nell'applicazione di modificare lo schema del database. Ad esempio, questo potrebbe verificarsi in un attacco SQL injection riuscito. Questo modello non viene usato da queste esercitazioni. Non si applica a SQL Server Compact e non si applica quando si esegue la migrazione a SQL Server in un'esercitazione successiva di questa serie. Il sito di Cytanium offre un solo account utente per accedere al database SQL Server creato in Cytanium. Se si è in grado di implementare questo modello nello scenario, è possibile eseguire questa operazione attenendosi alla procedura seguente:

1. Nella scheda **Impostazioni** della procedura guidata **Pubblica sito Web** , immettere la stringa di connessione che specifica un utente con autorizzazioni complete per l'aggiornamento dello schema del database e deselezionare la casella di controllo **Usa questa stringa di connessione in fase di esecuzione** . Il file Web. config distribuito diventa la stringa di connessione `DatabasePublish`.
2. Creare una trasformazione del file Web. config per la stringa di connessione che si desidera venga utilizzata dall'applicazione in fase di esecuzione.

A questo punto l'applicazione è stata distribuita in IIS nel computer di sviluppo ed è stata testata. In questo modo si verifica che il processo di distribuzione abbia copiato il contenuto dell'applicazione nel percorso corretto (esclusi i file che non si desidera distribuire) e che Distribuzione Web configurato correttamente IIS durante la distribuzione. Nell'esercitazione successiva verrà eseguito un ulteriore test per trovare un'attività di distribuzione che non è ancora stata eseguita: impostazione delle autorizzazioni per le cartelle nella cartella *ELMAH*

## <a name="more-information"></a>Altre informazioni

Per informazioni sull'esecuzione di IIS o IIS Express in Visual Studio, vedere le risorse seguenti:

- [IIS Express Panoramica](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sul sito IIS.NET.
- [Introduzione a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) nel Blog di Scott Guthrie.
- [Procedura: specificare il server Web per i progetti Web in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Differenze principali tra IIS e il server di sviluppo ASP.NET](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) nel sito di ASP.NET.
- [Testare l'applicazione ASP.NET MVC o Web Forms in IIS 7 in 30 secondi nel](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) Blog di Rick Anderson. Questa voce fornisce esempi del motivo per cui i test con il Server di sviluppo Visual Studio (Cassini) non sono affidabili come i test in IIS Express e perché i test in IIS Express non sono affidabili come i test in IIS.

Per informazioni sui problemi che possono verificarsi quando l'applicazione è in esecuzione in attendibilità media, vedere [hosting di applicazioni ASP.NET in un trust medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) nei 4 ragazzi dal sito Rolla.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
