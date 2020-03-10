---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione nell'ambiente di produzione-7 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568019"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: distribuzione nell'ambiente di produzione-7 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica

In questa esercitazione si configura un account con un provider di hosting e si distribuisce l'applicazione Web ASP.NET nell'ambiente di produzione usando la funzionalità di pubblicazione con un solo clic di Visual Studio.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Selezione di un provider di hosting

Per l'applicazione Contoso University e questa serie di esercitazioni, è necessario un provider che supporti ASP.NET 4 e Distribuzione Web. È stata scelta una società di hosting specifica in modo che le esercitazioni possano illustrare l'esperienza completa di distribuzione in un sito Web attivo. Ogni società di hosting fornisce funzionalità diverse e l'esperienza di distribuzione nei propri server varia leggermente. Tuttavia, il processo descritto in questa esercitazione è tipico per il processo complessivo. Il provider di hosting usato per questa esercitazione, Cytanium.com, è uno dei molti disponibili e il suo utilizzo in questa esercitazione non costituisce un'approvazione o una raccomandazione.

Quando si è pronti per selezionare un provider di hosting personalizzato, è possibile confrontare le funzionalità e i prezzi nella [raccolta di provider](https://www.microsoft.com/web/hosting) nel sito di Microsoft.com/Web.

## <a name="creating-an-account"></a>Creazione di un account

Creare un account presso il provider selezionato. Se il supporto per un database completo SQL Server è un ulteriore aggiunto, non è necessario selezionarlo per questa esercitazione, ma sarà necessario per l'esercitazione [migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) più avanti in questa serie.

Per queste esercitazioni, non è necessario registrare un nuovo nome di dominio. È possibile eseguire il test per verificare la corretta distribuzione utilizzando l'URL temporaneo assegnato al sito dal provider.

Dopo aver creato l'account, in genere si riceve un messaggio di posta elettronica di benvenuto contenente tutte le informazioni necessarie per distribuire e gestire il sito. Le informazioni inviate dal provider di hosting saranno simili a quelle illustrate qui. Il messaggio di posta elettronica di benvenuto Cytanium inviato ai nuovi proprietari dell'account include le informazioni seguenti:

- URL del sito del pannello di controllo del provider, in cui è possibile gestire le impostazioni per il sito. L'ID e la password specificati sono inclusi in questa parte del messaggio di posta elettronica di benvenuto per un riferimento semplice. Entrambi sono stati modificati in un valore demo per questa illustrazione.

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versione predefinita di .NET Framework e le informazioni su come modificarle. Molti siti di hosting vengono usati per impostazione predefinita su 2,0, che funziona con le applicazioni ASP.NET destinate a .NET Framework 2,0, 3,0 o 3,5. Tuttavia Contoso University è un'applicazione .NET Framework 4, quindi è necessario modificare questa impostazione. Per un'applicazione ASP.NET 4,5, è possibile usare l'impostazione di .NET 4,0.

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- URL temporaneo che è possibile utilizzare per accedere al sito Web. Quando l'account è stato creato, è stato immesso "contosouniversity.com" come nome di dominio esistente. L'URL temporaneo viene pertanto `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informazioni su come configurare i database e le stringhe di connessione necessarie per accedervi:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informazioni sugli strumenti e le impostazioni per la distribuzione del sito. Il messaggio di posta elettronica da Cytanium cita anche WebMatrix, che viene omesso qui.

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Impostazione della versione di .NET Framework

Il messaggio di benvenuto Cytanium include un collegamento alle istruzioni su come modificare la versione del .NET Framework. Queste istruzioni spiegano che questa operazione può essere eseguita tramite il pannello di controllo di Cytanium. Altri provider hanno siti del pannello di controllo che hanno un aspetto diverso oppure possono richiedere di eseguire questa operazione in modo diverso.

Passare all'URL del pannello di controllo. Dopo aver eseguito l'accesso con il nome utente e la password, viene visualizzato il pannello di controllo.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Nella casella **spazi host** , mantenere il puntatore sull'icona web e selezionare **siti Web** dal menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Nella casella **siti Web** fare clic su **contosouniversity.com** (il nome del sito usato al momento della creazione dell'account).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Nella casella **Proprietà sito Web** selezionare la scheda **estensioni** .

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Modificare ASP.NET dalla **pipeline integrata 2,0** a **4,0 (pipeline integrata)** , quindi fare clic su **Aggiorna**.

## <a name="publishing-to-the-hosting-provider"></a>Pubblicazione nel provider di hosting

Il messaggio di posta elettronica di benvenuto del provider di hosting include tutte le impostazioni necessarie per pubblicare il progetto ed è possibile immettere manualmente tali informazioni in un profilo di pubblicazione. Tuttavia, si userà un metodo più semplice e meno soggetto a errori per configurare la distribuzione nel provider: si scaricherà un file con *estensione publishsettings* e lo si importerà in un profilo di pubblicazione.

Nel browser passare al pannello di controllo di Cytanium e selezionare **Web** e quindi selezionare **siti Web.**

![Pannello di controllo selezione di siti Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Selezionare il sito Web **contosouniversity.com** .

![Pannello di controllo selezione di contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Selezionare la scheda **pubblicazione sul Web** .

![Scheda pubblicazione sul Web del pannello di controllo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Creazione di credenziali da utilizzare per la pubblicazione sul Web immettendo un nome utente e una password. È possibile immettere le stesse credenziali utilizzate per accedere al pannello di controllo. Fare quindi clic su **Attiva**.

![Pannello di controllo creare le credenziali di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Fare clic su **Scarica profilo di pubblicazione per il sito Web**.

![Profilo di pubblicazione del download del pannello di controllo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Quando viene richiesto di aprire o salvare il file, salvarlo.

![Salva file del profilo di pubblicazione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

In **Esplora soluzioni** in Visual Studio fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**. Verrà visualizzata la finestra di dialogo **Pubblica sito Web** nella scheda **Anteprima** con il profilo **test** selezionato perché è l'ultimo profilo utilizzato.

Selezionare la scheda **profilo** , quindi fare clic su **Importa**.

![Pulsante di importazione della procedura guidata Pubblica sito Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Nella finestra di dialogo **Importa impostazioni di pubblicazione** selezionare il file con *estensione publishsettings* scaricato e fare clic su **Apri**. La procedura guidata consente di passare alla scheda connessione con tutti i campi compilati.

![Scheda connessione guidata pubblicazione sito Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Il file con estensione publishsettings inserisce l'URL permanente pianificato per il sito nella casella URL di destinazione, ma se tale dominio non è ancora stato acquistato, sostituire il valore con l'URL temporaneo. Per questo esempio, l'URL è  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* L'unico scopo di questa casella è specificare quale URL verrà aperto automaticamente dal browser dopo la distribuzione. Se viene lasciato vuoto, l'unica conseguenza è che il browser non verrà avviato automaticamente dopo la distribuzione.

Fare clic su **convalida connessione** per verificare che le impostazioni siano corrette e che sia possibile connettersi al server. Come illustrato in precedenza, un segno di spunta verde verifica che la connessione abbia esito positivo.

Quando si fa clic su convalida connessione, potrebbe essere visualizzata una finestra di dialogo di **errore del certificato** . In tal caso, verificare che il nome del server sia quello previsto. In caso contrario, selezionare **Salva questo certificato per le sessioni future di Visual Studio** e fare clic su **accetta**. Questo errore indica che il provider di hosting ha scelto di evitare il costo di acquisto di un certificato SSL per l'URL in cui si esegue la distribuzione. Se si preferisce stabilire una connessione sicura usando un certificato valido, contattare il provider di hosting.

![Errore certificato](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Fare clic su **Avanti**.

Nella sezione **database** della scheda **Impostazioni** immettere gli stessi valori immessi per il profilo di pubblicazione del test. Sono disponibili le stringhe di connessione necessarie negli elenchi a discesa.

- Nella casella stringa di connessione per **schoolContext** selezionare `Data Source=|DataDirectory|School-Prod.sdf`
- In **schoolContext**selezionare **applica migrazioni Code First**.
- Nella casella stringa di connessione per **DefaultConnection**selezionare `Data Source=|DataDirectory|aspnet-Prod.sdf`
- In **DefaultConnection**lasciare deselezionata l' **aggiornamento del database** .

![Scheda Impostazioni della procedura guidata Pubblica sito Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Fare clic su **Avanti**.

Nella scheda **Anteprima** fare clic su **Avvia anteprima** per visualizzare un elenco dei file che verranno copiati. Viene visualizzato lo stesso elenco visualizzato in precedenza quando è stato distribuito in IIS nel computer locale.

Prima di pubblicare, modificare il nome del profilo in modo che il file di trasformazione Web. Production. config venga applicato. Selezionare la scheda **profilo** e fare clic su **Gestisci profili**.

![Procedura guidata Pubblica sito Web Gestisci profili](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Nella finestra di dialogo **modifica profili di pubblicazione Web** selezionare il profilo di produzione, fare clic su **Rinomina**e modificare il nome del profilo in produzione. e quindi fare clic su **Chiudi**.

![Finestra di dialogo Modifica profili di pubblicazione Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Fare clic su **Pubblica**.

L'applicazione viene pubblicata nel provider di hosting. Il risultato viene visualizzato nella finestra **output** .

![Finestra di output dopo la distribuzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Il browser si aprirà automaticamente con l'URL immesso nella casella **URL di destinazione** nella scheda **connessione** della procedura guidata **Pubblica sito Web** . Viene visualizzato lo stesso home page di quando si esegue il sito in Visual Studio, a meno che non sia presente un indicatore di ambiente "(test)" o "(dev)" nella barra del titolo. Ciò indica che la trasformazione *Web. config* dell'indicatore di ambiente ha funzionato correttamente.

> [!NOTE]
> Se nell'intestazione viene ancora visualizzato "(test)", eliminare la cartella *obj* dal progetto ContosoUniversity e ridistribuirla. Nelle versioni non definitive del software, il file di trasformazione applicato in precedenza (Web. test. config) potrebbe essere applicato di nuovo anche se si utilizza il profilo di produzione.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Prima di eseguire una pagina che causa l'accesso al database, verificare che ELMAH sia in grado di registrare gli eventuali errori che si verificano.

## <a name="setting-folder-permissions-for-elmah"></a>Impostazione delle autorizzazioni per le cartelle per ELMAH

Come è possibile ricordare dall'esercitazione precedente di questa serie, è necessario assicurarsi che l'applicazione disponga delle autorizzazioni di scrittura per la cartella nell'applicazione in cui ELMAH archivia i file di log degli errori. Quando si esegue la distribuzione in IIS in locale nel computer, queste autorizzazioni vengono impostate manualmente. In questa sezione si vedrà come impostare le autorizzazioni in Cytanium. È possibile che alcuni provider di hosting non consentano di eseguire questa operazione. possono offrire una o più cartelle predefinite con autorizzazioni di scrittura. In tal caso, è necessario modificare l'applicazione per usare le cartelle specificate.

È possibile impostare le autorizzazioni per le cartelle nel pannello di controllo di Cytanium. Passare all'URL del pannello di controllo e selezionare **file Manager**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Nella casella **file Manager** selezionare **contosouniversity.com** e quindi **wwwroot** per visualizzare la cartella radice dell'applicazione. Fare clic sull'icona del lucchetto accanto a **ELMAH**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Nella finestra **autorizzazioni** **file**/cartella selezionare le caselle di controllo **lettura** e **scrittura** per **contosouniversity.com** e fare clic su **Imposta autorizzazioni**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Verificare che ELMAH disponga dell'accesso in scrittura alla cartella *ELMAH* causando un errore e quindi visualizzando la segnalazione errori ELMAH. Richiedere un URL non valido, ad esempio *Studentsxxx. aspx*. Come in precedenza, viene visualizzata la pagina *pagina GenericErrorPage. aspx* . Fare clic sul collegamento **Disconnetti** , quindi eseguire *ELMAH. axd*. Si ottiene innanzitutto la pagina **di accesso** , che verifica che la trasformazione *Web. config* abbia aggiunto correttamente l'autorizzazione ELMAH. Dopo aver eseguito l'accesso, viene visualizzato il report in cui viene visualizzato l'errore appena generato.

[![ELMAH. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Test nell'ambiente di produzione

Eseguire la pagina **students** . L'applicazione tenta di accedere al database School per la prima volta, che attiva Migrazioni Code First per creare il database. Quando la pagina viene visualizzata dopo un breve intervallo di tempo, indica che non sono presenti studenti.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Eseguire la pagina **insegnanti** per verificare che i dati di inizializzazione abbiano inserito correttamente i dati dell'insegnante nel database.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Come nel caso dell'ambiente di test, si desidera verificare che gli aggiornamenti del database funzionino nell'ambiente di produzione, ma in genere non si desidera immettere i dati di test nel database di produzione. Per questa esercitazione si userà lo stesso metodo che è stato fatto in test. In un'applicazione reale, tuttavia, potrebbe essere necessario trovare un metodo per la convalida dell'esito positivo degli aggiornamenti del database senza introdurre i dati di test nel database di produzione. In alcune applicazioni, potrebbe essere utile aggiungere qualcosa e quindi eliminarlo.

Aggiungere uno studente e visualizzare i dati immessi nella pagina **students** per verificare che sia possibile aggiornare i dati nel database.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Verificare che le regole di autorizzazione funzionino correttamente selezionando **Aggiorna crediti** dal menu **corsi** . Verrà visualizzata la pagina **di accesso** . Immettere le credenziali dell'account amministratore, fare clic su **Accedi**. verrà visualizzata la pagina di **aggiornamento crediti** .

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Se l'accesso ha esito positivo, viene visualizzata la pagina di **aggiornamento crediti** . Ciò indica che il database di appartenenza ASP.NET (con l'account amministratore singolo) è stato distribuito correttamente.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Il sito è stato correttamente distribuito e testato ed è disponibile pubblicamente su Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Creazione di un ambiente di test più affidabile

Come spiegato nell'esercitazione [distribuzione nell'ambiente di testing](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) , l'ambiente di test più affidabile sarebbe un secondo account del provider di hosting che è analogo all'account di produzione. Questa operazione sarebbe più costosa rispetto all'uso di IIS locale come ambiente di test, poiché sarebbe necessario iscriversi per un secondo account di hosting. Tuttavia, se impedisce errori o interruzioni del sito di produzione, è possibile che si richiedano i costi.

La maggior parte del processo di creazione e distribuzione in un account di test è simile a quanto già fatto per la distribuzione nell'ambiente di produzione:

- Creare un file di trasformazione di *Web. config* .
- Creare un account nel provider di hosting.
- Consente di creare un nuovo profilo di pubblicazione e di distribuirlo all'account di test.

### <a name="preventing-public-access-to-the-test-site"></a>Prevenzione dell'accesso pubblico al sito di test

Una considerazione importante per l'account di test è che sarà disponibile su Internet, ma non si vuole che il pubblico lo usi. Per rendere privato il sito, è possibile usare uno o più dei metodi seguenti:

- Contattare il provider di hosting per impostare le regole del firewall che consentono l'accesso al sito di testing solo da indirizzi IP usati per il testing.
- Mascherare l'URL in modo che non sia simile all'URL del sito pubblico.
- Usare un file *robots. txt* per assicurarsi che i motori di ricerca non eseguiranno la ricerca per indicizzazione del sito di test e i collegamenti ai report nei risultati della ricerca.

Il primo di questi metodi è ovviamente il più sicuro, ma la procedura per è specifica di ogni provider di hosting e non verrà analizzata in questa esercitazione. Se si dispone del provider di hosting per consentire solo all'indirizzo IP di passare all'URL dell'account di test, in teoria non è necessario preoccuparsi dei motori di ricerca che eseguono la ricerca per indicizzazione. Tuttavia, anche in questo caso, la distribuzione di un file *robots. txt* è una scelta ideale come backup nel caso in cui la regola del firewall venga accidentalmente spenta.

Il file *robots. txt* viene inserito nella cartella del progetto e deve avere il testo seguente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

La riga `User-agent` indica ai motori di ricerca che le regole del file si applicano a tutti i crawler web (robot) del motore di ricerca e la riga `Disallow` specifica che non deve essere eseguita la ricerca per indicizzazione nelle pagine del sito.

Probabilmente si vuole che i motori di ricerca cataloghino il sito di produzione, quindi è necessario escludere questo file dalla distribuzione di produzione. A tale scopo, vedere è **possibile escludere cartelle o file specifici dalla distribuzione?** in [domande frequenti sulla distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Assicurarsi di specificare l'esclusione solo per il profilo di pubblicazione di produzione.

La creazione di un secondo account di hosting è un approccio all'utilizzo di un ambiente di test che non è necessario, ma potrebbe valere la spesa aggiuntiva. Nelle esercitazioni seguenti si continuerà a usare IIS come ambiente di test.

Nell'esercitazione successiva si aggiornerà il codice dell'applicazione e si distribuirà la modifica negli ambienti di test e di produzione.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
