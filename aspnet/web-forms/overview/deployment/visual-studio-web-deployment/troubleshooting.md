---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Distribuzione Web ASP.NET con Visual Studio: risoluzione dei problemi | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b42476fca18b04f4557a216ee205cfd9220023e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576097"
---
# <a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Distribuzione Web ASP.NET con Visual Studio: risoluzione dei problemi

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

Questa pagina descrive alcuni problemi comuni che possono verificarsi quando si distribuisce un'applicazione Web ASP.NET con Visual Studio. Per ognuna di esse sono disponibili una o più possibili cause e le soluzioni corrispondenti.

Gli scenari illustrati si applicano sia ai provider di hosting di Azure che di terze parti. Per altre informazioni sulla risoluzione dei problemi di app Web in Servizio app di Azure, vedere le risorse seguenti:

- [Risoluzione dei problemi di un'app Web nel servizio app di Azure tramite Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Eseguire il monitoraggio delle app Web nel servizio app di Azure](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Annunciando la versione di Windows Azure SDK 2,0 per .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Blog di ScottGu, viene illustrato come ottenere i log di diagnostica in Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Errore del server nell'applicazione '/'-le impostazioni degli errori personalizzati correnti impediscono la visualizzazione remota dei dettagli dell'errore

### <a name="scenario"></a>Scenario

Dopo la distribuzione di un sito in un host remoto, viene ricevuto un messaggio di errore che indica l'impostazione customErrors nel file Web. config, ma non indica la vera e propria origine dell'errore:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Per impostazione predefinita, ASP.NET Visualizza informazioni dettagliate sugli errori solo quando l'applicazione Web è in esecuzione nel computer locale. In genere non si vogliono visualizzare informazioni dettagliate sugli errori quando l'applicazione Web è disponibile pubblicamente su Internet, perché gli hacker potrebbero essere in grado di usare queste informazioni per individuare le vulnerabilità nell'applicazione. Tuttavia, quando si distribuisce un sito o gli aggiornamenti in un sito, a volte si verifica un errore ed è necessario ottenere il messaggio di errore effettivo.

Per consentire all'applicazione di visualizzare messaggi di errore dettagliati durante l'esecuzione nell'host remoto, modificare il file Web. config per impostare la modalità customErrors su off, ridistribuire l'applicazione ed eseguire di nuovo l'applicazione:

1. Se il file Web. config dell'applicazione contiene un elemento customErrors nell'elemento System. Web, impostare l'attributo mode su "off". In caso contrario, aggiungere un elemento customErrors nell'elemento System. Web con l'attributo mode impostato su "off", come illustrato nell'esempio seguente: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Distribuire l'applicazione.
3. Eseguire l'applicazione e ripetere qualsiasi operazione eseguita in precedenza che ha causato l'errore. A questo punto è possibile visualizzare il messaggio di errore effettivo.
4. Dopo aver risolto l'errore, ripristinare l'impostazione customErrors originale e ridistribuire l'applicazione.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Non è possibile creare una copia shadow ' ContosoUniversity ' quando il file esiste già.

### <a name="scenario"></a>Scenario

Quando si tenta di eseguire un progetto in Visual Studio, viene visualizzata una pagina di errore con un messaggio simile all'esempio seguente:

Errore del server nell'applicazione '/'. Non è possibile creare una copia shadow ' ContosoUniversity ' quando il file esiste già.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Attendere un minuto e aggiornare il browser oppure ricompilare il sito e provare a eseguirlo di nuovo.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Accesso negato in una pagina Web che usa SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si distribuisce un sito che utilizza SQL Server Compact e si esegue una pagina nel sito distribuito che accede al database, viene visualizzato il messaggio di errore seguente:

Accesso negato. (Eccezione da HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

L'account servizio di rete nel server deve essere in grado di leggere i file binari nativi di SQL Service Compact presenti nella cartella *bin\amd64* o *bin\x86* , ma non dispone delle autorizzazioni di lettura per tali cartelle. Impostare l'autorizzazione di lettura per servizio di rete nella cartella *bin* , assicurandosi di estendere le autorizzazioni alle sottocartelle.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Impossibile leggere il file di configurazione a causa di autorizzazioni insufficienti

### <a name="scenario"></a>Scenario

Quando si fa clic sul pulsante pubblica di Visual Studio per distribuire un'applicazione in IIS nel computer locale, la pubblicazione non riesce e nella finestra di **output** viene visualizzato un messaggio di errore simile al seguente:

Si è verificato un errore durante la lettura del file di configurazione IIS "computer/Reindirizzamento". L'identità che esegue questa operazione è... Errore: Impossibile leggere il file di configurazione a causa di autorizzazioni insufficienti.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Per usare la pubblicazione con un clic in IIS nel computer locale, è necessario eseguire Visual Studio con le autorizzazioni di amministratore. Chiudere Visual Studio e riavviarlo con le autorizzazioni di amministratore.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Impossibile connettersi al computer di destinazione... Uso del processo specificato

### <a name="scenario"></a>Scenario

Quando si fa clic sul pulsante pubblica di Visual Studio per distribuire un'applicazione, la pubblicazione non riesce e nella finestra di **output** viene visualizzato un messaggio di errore simile al seguente:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Un server proxy interrompe la comunicazione con il server di destinazione. Nel pannello di controllo di Windows o in Internet Explorer selezionare **Opzioni Internet** e selezionare la scheda **connessioni** . Nella finestra di dialogo **Proprietà Internet** fare clic su **Impostazioni LAN**. Nella finestra di dialogo **Impostazioni rete locale (LAN)** deselezionare la casella di controllo **rileva automaticamente impostazioni** . Quindi fare di nuovo clic sul pulsante pubblica.

Se il problema persiste, contattare l'amministratore di sistema per determinare le operazioni che possono essere eseguite con le impostazioni del proxy o del firewall. Il problema si verifica perché Distribuzione Web usa una porta non standard per la distribuzione del servizio di gestione Web (8172); per altre connessioni, Distribuzione Web usa la porta 80. Quando si esegue la distribuzione in un provider di hosting di terze parti, si utilizza in genere il servizio gestione Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Il pool di applicazioni .NET 4,0 predefinito non esiste

### <a name="scenario"></a>Scenario

Quando si distribuisce un'applicazione che richiede il .NET Framework 4, viene visualizzato il messaggio di errore seguente:

Il pool di applicazioni .NET 4,0 predefinito non esiste oppure non è stato possibile aggiungere l'applicazione. Verificare che ASP.NET 4,0 sia installato nel computer.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

ASP.NET 4 non è installato in IIS. Se il server in cui si esegue la distribuzione è il computer di sviluppo in cui è installato Visual Studio 2010, ASP.NET 4 è installato nel computer, ma potrebbe non essere installato in IIS. Nel server in cui si esegue la distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Potrebbe anche essere necessario impostare manualmente la versione .NET Framework del pool di applicazioni predefinito. Per ulteriori informazioni, vedere l'esercitazione distribuzione in IIS come ambiente di testing in questa serie.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Il formato della stringa di inizializzazione non è conforme alla specifica a partire dall'indice 0.

### <a name="scenario"></a>Scenario

Dopo aver distribuito un'applicazione utilizzando la pubblicazione con un clic, quando si esegue una pagina che accede al database viene ricevuto il messaggio di errore seguente:

Il formato della stringa di inizializzazione non è conforme alla specifica a partire dall'indice 0.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Aprire il file *Web. config* nel sito distribuito e verificare se i valori della stringa di connessione iniziano con `$(ReplaceableToken_`, come nell'esempio seguente:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Se le stringhe di connessione sono simili a questo esempio, modificare il file di progetto e aggiungere la proprietà seguente all'elemento PropertyGroup per tutte le configurazioni di compilazione:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Quindi ridistribuire l'applicazione.

## <a name="http-500-internal-server-error"></a>Errore interno del server HTTP 500

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, viene visualizzato il messaggio di errore seguente senza informazioni specifiche che indicano la provocazione dell'errore:

Errore HTTP 500-errore interno del server.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Ci sono molte cause di 500 errori, ma una possibile causa se si seguono queste esercitazioni è che si inserisce un elemento XML nella posizione errata in uno dei file di trasformazione Web. config. Se, ad esempio, si inserisce una trasformazione che inserisce un &lt;percorso&gt; elemento in &lt;System. Web&gt; anziché direttamente in &lt;&gt;di configurazione. È possibile utilizzare la funzionalità di anteprima della trasformazione Web. config per verificare che le trasformazioni funzionino come previsto. La soluzione se si individua una trasformazione codificata in modo errato, è necessario correggere il file di trasformazione e ridistribuirlo. Se un errore non è evidente, provare a impostare come commento le trasformazioni e la ridistribuzione per individuare la causa dell'errore 500.

## <a name="http-50021-internal-server-error"></a>Errore interno del server HTTP 500,21

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, viene visualizzato il messaggio di errore seguente:

Errore HTTP 500,21-errore interno del server. Il gestore "PageHandlerFactory-Integrated" ha un modulo "ManagedPipelineHandler" non valido nell'elenco dei moduli.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Il sito distribuito è destinato a ASP.NET 4, ma ASP.NET 4 non è registrato in IIS nel server. Sul server aprire un prompt dei comandi con privilegi elevati e registrare ASP.NET 4 eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Potrebbe anche essere necessario impostare manualmente la versione .NET Framework del pool di applicazioni predefinito. Per ulteriori informazioni, vedere l'esercitazione distribuzione in IIS come ambiente di testing in questa serie.

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Accesso non riuscito durante l'apertura del database SQL Server Express nei dati dell'app\_

### <a name="scenario"></a>Scenario

La stringa di connessione al file *Web. config* è stata aggiornata in modo da puntare a un database SQL Server Express come file con *estensione mdf* nell' *app\_cartella dati* e la prima volta che si esegue l'applicazione viene visualizzato il messaggio di errore seguente:

System. Data. SqlClient. SqlException: Impossibile aprire il database "DatabaseName" richiesto dall'account di accesso. Accesso non riuscito.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Il nome del file con *estensione MDF* non può corrispondere al nome di un database SQL Server Express esistente nel computer, anche se è stato eliminato il file con *estensione MDF* del database precedentemente esistente. Modificare il nome del file con *estensione MDF* in un nome che non sia mai stato utilizzato come nome di database e modificare il file *Web. config* in modo da utilizzare il nuovo nome. In alternativa, è possibile utilizzare [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) per eliminare i database di SQL Server Express esistenti in precedenza.

## <a name="model-compatibility-cannot-be-checked"></a>Impossibile controllare la compatibilità del modello

### <a name="scenario"></a>Scenario

È stata aggiornata la stringa di connessione al file *Web. config* in modo che punti a un nuovo database di SQL Server Express e la prima volta che si esegue l'applicazione viene visualizzato il messaggio di errore seguente:

Impossibile controllare la compatibilità del modello perché il database non contiene metadati del modello. Assicurarsi che IncludeMetadataConvention sia stato aggiunto alle convenzioni di DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Se il nome del database inserito nel file Web. config era mai stato utilizzato prima nel computer, è possibile che esista già un database con alcune tabelle. Selezionare un nuovo nome che non sia stato usato nel computer prima e modificare il file *Web. config* in modo che punti a usare il nuovo nome del database. In alternativa, è possibile usare [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) per eliminare il database esistente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Errore SQL quando uno script tenta di creare utenti o ruoli

### <a name="scenario"></a>Scenario

Si sta usando la distribuzione del database configurata nella scheda **Pubblicazione/creazione pacchetto SQL** , gli script SQL che vengono eseguiti durante la distribuzione includono i comandi Crea utente o Crea ruolo e l'esecuzione dello script non riesce quando vengono eseguiti questi comandi. È possibile che vengano visualizzati messaggi più dettagliati, come i seguenti:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Se questo errore si verifica quando si configura la distribuzione del database nella procedura guidata **Pubblica sito Web** anziché nella scheda **Pubblicazione/creazione pacchetto SQL** , creare un thread nel forum di [configurazione e distribuzione](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) e la soluzione verrà aggiunta a questa pagina di risoluzione dei problemi.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

L'account utente utilizzato per eseguire la distribuzione non dispone dell'autorizzazione per la creazione di utenti o ruoli. Ad esempio, la società di hosting potrebbe assegnare i ruoli DB\_DataReader, DB\_DataWriter e DB\_ddladmin all'account utente che configura. Sono sufficienti per la creazione della maggior parte degli oggetti di database, ma non per la creazione di utenti o ruoli. Un modo per evitare l'errore è escludere gli utenti e i ruoli dalla distribuzione del database. A tale scopo, è possibile modificare l'elemento PreSource per lo script generato automaticamente del database in modo che includa gli attributi seguenti:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Per informazioni su come modificare l'elemento PreSource nel file di progetto, vedere [procedura: modificare le impostazioni di distribuzione nel file di progetto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Se gli utenti o i ruoli nel database di sviluppo devono trovarsi nel database di destinazione, rivolgersi al provider di hosting per assistenza.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Errore SQL Server timeout durante l'esecuzione di script personalizzati durante la distribuzione

### <a name="scenario"></a>Scenario

Sono stati specificati script SQL personalizzati da eseguire durante la distribuzione e quando Distribuzione Web li esegue, si verifica il timeout.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

L'esecuzione di più script con modalità di transazione diverse può causare errori di timeout. Per impostazione predefinita, gli script generati automaticamente vengono eseguiti in una transazione, ma non negli script personalizzati. Se si seleziona l'opzione **Estrai dati e/o schema da un database esistente** nella scheda **Pubblicazione/creazione pacchetto SQL** e si aggiunge uno script SQL personalizzato, è necessario modificare le impostazioni delle transazioni in alcuni script in modo che tutti gli script usino le stesse impostazioni di transazione. Per altre informazioni, vedere [procedura: distribuire un database con un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343.aspx).

Se sono state configurate le impostazioni delle transazioni in modo che tutte siano uguali ma si ottengano comunque questo errore, una possibile soluzione alternativa consiste nell'eseguire gli script separatamente. Nella griglia **script di database** della scheda **Pubblicazione/creazione pacchetto** SQL deselezionare la casella di controllo **Includi** per lo script che causa l'errore di timeout, quindi pubblicare il progetto. Tornare quindi alla griglia degli **script di database** , selezionare la casella di controllo **Includi** script e deselezionare le caselle di controllo **Includi** per gli altri script. Quindi pubblicare nuovamente il progetto. Questa volta, quando si pubblica, viene eseguito solo lo script personalizzato selezionato.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>I dati del flusso del manifesto del sito non sono ancora disponibili

### <a name="scenario"></a>Scenario

Quando si installa un pacchetto usando il file *deploy. cmd* con l'opzione t (test), viene visualizzato il messaggio di errore seguente:

Errore: i dati del flusso di ' sitemanifest/dbFullSql [@path=' C:\TEMP\AdventureWorksGrant.sql ']/sqlScript ' non sono ancora disponibili.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Il messaggio di errore indica che il comando non può produrre un report di test. Tuttavia, il comando può essere eseguito se si usa l'opzione y (Actual Installation). Il messaggio indica solo che si è verificato un problema durante l'esecuzione del comando in modalità test.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Questa applicazione richiede ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Scenario

Quando si tenta di distribuire, viene visualizzato il messaggio di errore seguente:

Il pool di applicazioni che si sta tentando di usare ha la proprietà' managedRuntimeVersion ' impostata su' v 2.0'. Questa applicazione richiede "v 4.0".

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

ASP.NET 4 non è installato in IIS. Se il server in cui si esegue la distribuzione è il computer di sviluppo in cui è installato Visual Studio 2010, ASP.NET 4 è installato nel computer, ma potrebbe non essere installato in IIS. Nel server in cui si esegue la distribuzione, aprire un prompt dei comandi con privilegi elevati e installare ASP.NET 4 in IIS eseguendo i comandi seguenti:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Non è possibile eseguire il cast di Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Scenario

Quando si distribuisce un pacchetto, viene visualizzato il messaggio di errore seguente:

Non è possibile eseguire il cast dell'oggetto di tipo ' Microsoft. Web. Deployment. DeploymentProviderOptions ' in ' Microsoft. Web. Deployment. DeploymentProviderOptions '.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Si sta tentando di eseguire la distribuzione da Gestione IIS usando l'interfaccia utente di Distribuzione Web 1,1 in un server in cui è installato Distribuzione Web 2,0. Se si utilizza lo strumento di amministrazione remota IIS per eseguire la distribuzione importando un pacchetto, selezionare la finestra di dialogo **nuove funzionalità disponibili** quando si stabilisce la connessione. Questa finestra di dialogo può essere visualizzata una sola volta quando la connessione viene stabilita per la prima volta. Per cancellare la connessione e ricominciare, chiudere Gestione IIS e riavviarlo immettendo inetmgr/reset al prompt dei comandi. Se una delle funzionalità elencate è **distribuzione Web interfaccia utente**e il numero di versione è inferiore a 8, nel server in cui si esegue la distribuzione potrebbero essere installate entrambe le versioni 1,1 e 2,0 di distribuzione Web. Per eseguire la distribuzione da un client in cui è installato 2,0, è necessario che nel server sia installato solo Distribuzione Web 2,0. Per risolvere il problema, sarà necessario contattare il provider di hosting.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Impossibile caricare i componenti nativi di SQL Server Compact

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito, viene visualizzato il messaggio di errore seguente:

Non è possibile caricare i componenti nativi di SQL Server Compact corrispondenti al provider ADO.NET della versione 8482. Installare la versione corretta di SQL Server Compact. Per ulteriori informazioni, fare riferimento all'articolo della Knowledge 974247 KB.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Il sito distribuito non dispone di sottocartelle *amd64* e *x86* con gli assembly nativi presenti nella cartella *bin* dell'applicazione. In un computer in cui è installato SQL Server Compact, gli assembly nativi si trovano in *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private*. Il modo migliore per ottenere i file corretti nelle cartelle corrette in un progetto di Visual Studio consiste nell'installare il pacchetto NuGet SqlServerCompact. L'installazione del pacchetto aggiunge uno script di post-compilazione per copiare gli assembly nativi in *amd64* e *x86*. Per poter distribuire questi elementi, è tuttavia necessario includerli manualmente nel progetto. Per ulteriori informazioni, vedere l'esercitazione sulla [distribuzione di SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Errore "percorso non valido" dopo la distribuzione di un'applicazione Code First Entity Framework

### <a name="scenario"></a>Scenario

Si distribuisce un'applicazione che usa Migrazioni Code First di Entity Framework e un sistema DBMS, ad esempio SQL Server Compact che archivia il proprio database in un file nella cartella app\_data. È Migrazioni Code First configurato per creare il database dopo la prima distribuzione. Quando si esegue l'applicazione, viene ricevuto un messaggio di errore simile all'esempio seguente:

Il percorso non è valido. Controllare la directory per il database. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Code First sta tentando di creare il database ma la cartella app\_data non esiste. Non sono presenti file nella cartella *app\_data* durante la distribuzione oppure è stata selezionata l'opzione **escludi dati\_app** nella scheda **pubblicazione/pubblicazione del pacchetto Web** del progetto finestra Proprietà. Il processo di distribuzione non creerà una cartella nel server se non sono presenti file nella cartella da copiare nel server. Se il database è già stato configurato nel sito, il processo di distribuzione eliminerà i file e la cartella di *dati dell'App\_* se è stata selezionata l'opzione **Rimuovi file aggiuntivi nella destinazione** nel profilo di pubblicazione. Per risolvere il problema, inserire un file segnaposto, ad esempio un file con estensione txt, nella cartella *app\_data* , assicurarsi che non siano stati selezionati i **dati di esclusione\_app** e ridistribuire.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>Impossibile utilizzare "oggetto COM separato dall'RCW sottostante".

### <a name="scenario"></a>Scenario

La pubblicazione con un clic è stata eseguita correttamente per distribuire l'applicazione e quindi si inizia a ricevere questo errore:

Attività di distribuzione Web non riuscita. Non è stato possibile completare la richiesta all'URL dell'agente remoto '<https://serverurl.com/msdeploy.axd?site=sitename>'.  
 Non è stato possibile completare la richiesta all'URL dell'agente remoto '<https://url/msdeploy.axd?site=sitename>'.  
La richiesta è stata interrotta: la richiesta è stata annullata.  
Impossibile utilizzare l'oggetto COM separato dall'RCW sottostante.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

La chiusura e il riavvio di Visual Studio sono in genere tutti necessari per risolvere l'errore.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>La distribuzione non riesce perché le credenziali utente usate per la pubblicazione non hanno l'autorità setACL

### <a name="scenario"></a>Scenario

La pubblicazione ha esito negativo con un errore che indica che non si dispone dell'autorizzazione per impostare le autorizzazioni per le cartelle (l'account utente in uso non dispone dell'autorità setACL).

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Per impostazione predefinita, Visual Studio imposta le autorizzazioni di lettura per la cartella radice del sito e le autorizzazioni di scrittura nella cartella app\_data. Se è noto che le autorizzazioni predefinite per le cartelle del sito sono corrette e non è necessario impostarle, disabilitare questo comportamento aggiungendo **&lt;destinazione IncludeSetACLProviderOn&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** al file del profilo di pubblicazione (per influire su un solo profilo) o al file WPP. targets (per influire su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nei file di profilo (con estensione pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errori di accesso negato quando l'applicazione tenta di scrivere in una cartella dell'applicazione

### <a name="scenario"></a>Scenario

Si verificano errori dell'applicazione quando si tenta di creare o modificare un file in una delle cartelle dell'applicazione, perché non dispone dell'autorità di scrittura per la cartella.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Per impostazione predefinita, Visual Studio imposta le autorizzazioni di lettura per la cartella radice del sito e le autorizzazioni di scrittura nella cartella app\_data. Se l'applicazione richiede l'accesso in scrittura a una sottocartella, è possibile impostare le autorizzazioni per tale cartella, come illustrato nelle esercitazioni impostazione autorizzazioni cartella e distribuzione in ambiente di produzione in questa serie. Se l'applicazione richiede l'accesso in scrittura alla cartella radice del sito, è necessario impedirne l'impostazione dell'accesso in sola lettura nella cartella radice aggiungendo **&lt;destinazione IncludeSetACLProviderOn&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** al file del profilo di pubblicazione (per influire su un solo profilo) o al file WPP. targets (per influire su tutti i profili). Per informazioni su come modificare questi file, vedere [procedura: modificare le impostazioni di distribuzione nei file di profilo (con estensione pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Errore di configurazione-l'attributo targetFramework fa riferimento a una versione successiva alla versione installata del .NET Framework

### <a name="scenario"></a>Scenario

È stato pubblicato un progetto Web destinato a ASP.NET 4,5, ma quando si esegue l'applicazione (con la modalità customErrors impostata su "off" nel file Web. config) si verifica l'errore seguente:

L'attributo ' targetFramework ' nell'elemento di compilazione &lt;&gt; del file Web. config viene usato solo per la versione 4,0 e successive del .NET Framework (ad esempio '&lt;compilation targetFramework = "4.0"&gt;'). L'attributo ' targetFramework ' fa attualmente riferimento a una versione successiva alla versione installata del .NET Framework. Specificare una versione di destinazione valida del .NET Framework oppure installare la versione richiesta del .NET Framework.

Nella casella errore di origine della pagina di errore viene evidenziata la seguente riga da Web. config come causa dell'errore:

&lt;compilation targetFramework = "4.5"/&gt;

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Il server non supporta ASP.NET 4,5. Contattare il provider di hosting per determinare quando e se è possibile aggiungere il supporto per ASP.NET 4,5. Se l'aggiornamento del server non è un'opzione, è necessario distribuire un progetto Web destinato a ASP.NET 4 o versioni precedenti.

Se si distribuisce un progetto Web ASP.NET 4 o versione precedente nella stessa destinazione, selezionare la casella di controllo **Rimuovi file aggiuntivi nella destinazione** nella scheda **Impostazioni** della procedura guidata **Pubblica sito Web** . Se non si seleziona **Rimuovi file aggiuntivi nella destinazione**, si continuerà a ottenere la pagina di errore di configurazione.

Nelle finestre delle **Proprietà** del progetto è incluso un elenco a discesa Framework di destinazione, ma non è possibile risolvere il problema modificando il valore da **.NET Framework 4,5** al **.NET Framework 4**. Se si modifica il Framework di destinazione in una versione precedente del Framework, il progetto avrà ancora riferimenti agli assembly della versione del Framework successiva e non sarà eseguito. È necessario modificare manualmente i riferimenti o creare un nuovo progetto destinato a .NET Framework 4 o versioni precedenti. Per ulteriori informazioni, vedere [.NET Framework destinazione per i siti Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Errori di attendibilità media

### <a name="scenario"></a>Scenario

Quando l'applicazione viene eseguita nell'ambiente di produzione, si verifica un errore correlato all'attendibilità media.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

Molti provider di hosting di terze parti eseguono il sito Web in attendibilità media, il che significa che non è consentito eseguire alcune operazioni. Ad esempio, il codice dell'applicazione non riesce ad accedere al registro di sistema di Windows e non è in grado di leggere o scrivere file esterni alla gerarchia di cartelle dell'applicazione. Per impostazione predefinita, l'applicazione viene eseguita con *attendibilità totale* nel computer locale, il che significa che l'applicazione potrebbe essere in grado di eseguire operazioni che potrebbero avere esito negativo quando viene distribuita nell'ambiente di produzione.

Per la risoluzione dei problemi, è possibile configurare l'applicazione per l'esecuzione con attendibilità media nell'ambiente IIS locale. A tale scopo, aprire il file *Web. config* dell'applicazione e aggiungere un elemento **trust** nell'elemento **System. Web** , come illustrato in questo esempio.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

L'applicazione verrà ora eseguita in attendibilità media in IIS anche nel computer locale.

Non eseguire questa operazione se si esegue la distribuzione nel servizio app Azure, perché Azure non richiede attendibilità media. Al momento della scrittura di questa esercitazione nel febbraio 2012, l'uso di questo metodo per far sì che l'applicazione venga eseguita in attendibilità media provocherà un errore in Azure.

Se si usa Migrazioni Code First di Entity Framework e si distribuisce in un provider di hosting che esegue l'applicazione in attendibilità media, verificare che sia installata la versione 5,0 o successiva. In Entity Framework versione 4,3, le migrazioni richiedono l'attendibilità totale per aggiornare lo schema del database.

## <a name="http-40417-not-found-error"></a>Errore HTTP 404,17 non trovato

### <a name="scenario"></a>Scenario

Quando si esegue il sito distribuito nel computer di sviluppo in IIS, viene visualizzato il messaggio di errore seguente che segnala che il server non è in grado di elaborare default. aspx:

Errore HTTP 404,17-non trovato

Il contenuto richiesto sembra essere uno script e non verrà servito dal gestore di file statici.

### <a name="possible-cause-and-solution"></a>Possibili cause e soluzioni

ASP.NET 4,5 potrebbe non essere installato nel computer. Vedere i passaggi nell'esercitazione distribuzione in IIS come ambiente di testing in questa serie che illustra come installare ASP.NET 4,5.

> [!div class="step-by-step"]
> [Precedente](deploying-extra-files.md)
