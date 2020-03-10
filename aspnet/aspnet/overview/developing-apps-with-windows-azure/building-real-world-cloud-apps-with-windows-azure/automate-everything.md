---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizzare tutto (creazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584945"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizzare tutto (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per un'introduzione all'e-book, vedere [il primo capitolo](introduction.md).

I primi tre modelli esaminati si applicano effettivamente a qualsiasi progetto di sviluppo software, ma soprattutto ai progetti cloud. Questo modello riguarda l'automazione delle attività di sviluppo. Si tratta di un argomento importante perché i processi manuali sono lenti e soggette a errori; l'automazione del maggior numero possibile consente di configurare un flusso di lavoro rapido, affidabile e agile. È particolarmente importante per lo sviluppo nel cloud, perché consente di automatizzare facilmente molte attività difficili o impossibili da automatizzare in un ambiente locale. Ad esempio, è possibile configurare interi ambienti di test, tra cui nuovo server Web e VM back-end, database, archiviazione BLOB (archiviazione file), code e così via.

## <a name="devops-workflow"></a>Flusso di lavoro DevOps

Sempre più si sente il termine "DevOps". Il termine sviluppato da un riconoscimento necessario per integrare le attività di sviluppo e operazioni per sviluppare il software in modo efficiente. Il tipo di flusso di lavoro che si vuole abilitare è quello in cui è possibile sviluppare un'app, distribuirla, apprendere dall'utilizzo di produzione, modificarla in risposta a quanto appreso e ripetere il ciclo in modo rapido e affidabile.

Alcuni team di sviluppo cloud riusciti distribuiscono più volte al giorno in un ambiente Live. Il team di Azure ha usato per distribuire un aggiornamento principale ogni 2-3 mesi, ma ora rilascia aggiornamenti secondari ogni 2-3 giorni e le versioni principali ogni 2-3 settimane. Ottenere tale cadenza consente di rispondere ai commenti dei clienti.

A tale scopo, è necessario abilitare un ciclo di sviluppo e distribuzione ripetibile, affidabile, prevedibile e con tempi di ciclo ridotti.

![Flusso di lavoro DevOps](automate-everything/_static/image1.png)

In altre parole, il periodo di tempo che intercorre tra l'idea di una funzionalità e il momento in cui i clienti lo usano e l'invio di commenti e suggerimenti deve essere il più breve possibile. I primi tre modelli: automatizzare tutti gli elementi, il controllo del codice sorgente e l'integrazione e il recapito continui. si tratta di procedure consigliate consigliate per consentire questo tipo di processo.

## <a name="azure-management-scripts"></a>Script di gestione di Azure

Nell' [Introduzione a questo e-book](introduction.md)è stata illustrata la console basata sul Web, il portale di gestione di Azure. Il portale di gestione consente di monitorare e gestire tutte le risorse distribuite in Azure. Si tratta di un modo semplice per creare ed eliminare servizi quali app Web e VM, configurare tali servizi, monitorare le operazioni del servizio e così via. Si tratta di uno strumento straordinario, ma è un processo manuale. Se si intende sviluppare un'applicazione di produzione di qualsiasi dimensione e soprattutto in un ambiente team, è consigliabile passare attraverso l'interfaccia utente del portale per apprendere ed esplorare Azure e quindi automatizzare i processi che verranno ripetuti.

Quasi tutte le operazioni che è possibile eseguire manualmente nel portale di gestione o in Visual Studio possono essere eseguite anche chiamando l'API REST di gestione. È possibile scrivere script con [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)oppure usare un framework open source, ad esempio [chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet). È anche possibile usare lo strumento da riga di comando bash in un ambiente Mac o Linux. Azure dispone di API di scripting per tutti questi ambienti diversi ed è dotato di un' [API di gestione .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) nel caso in cui si desideri scrivere codice anziché script.

Per l'app Correggi it sono stati creati alcuni script di Windows PowerShell che consentono di automatizzare i processi di creazione di un ambiente di test e la distribuzione del progetto in tale ambiente. verrà esaminato parte del contenuto di tali script.

## <a name="environment-creation-script"></a>Script di creazione dell'ambiente

Il primo script da esaminare è denominato *New-AzureWebsiteEnv. ps1*. Viene creato un ambiente Azure in cui è possibile distribuire l'app per la correzione a per il test. Le attività principali eseguite dallo script sono le seguenti:

- Creare un'app Web.
- Creare un account di archiviazione. (Obbligatorio per i BLOB e le code, come si vedrà nei capitoli successivi).
- Creare un server di database SQL e due database: un database dell'applicazione e un database delle appartenenze.
- Archiviare le impostazioni in Azure che l'app userà per accedere all'account di archiviazione e ai database.
- Creare i file di impostazioni che verranno usati per automatizzare la distribuzione.

### <a name="run-the-script"></a>Esecuzione dello script

> [!NOTE]
> Questa parte del capitolo mostra esempi di script e i comandi da immettere per eseguirli. Questa demo non fornisce tutte le informazioni necessarie per eseguire gli script. Per istruzioni dettagliate su come eseguire l'operazione, vedere [Appendice: l'applicazione di esempio Fix it](the-fix-it-sample-application.md#deploybase).

Per eseguire uno script di PowerShell che gestisce i servizi di Azure, è necessario installare la console di Azure PowerShell e configurarla per l'uso con la sottoscrizione di Azure. Una volta configurato, è possibile eseguire lo script Fix it Environment Creation con un comando simile al seguente:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Il parametro `Name` specifica il nome da usare quando si creano gli account di archiviazione e del database, mentre il parametro `SqlDatabasePassword` specifica la password per l'account amministratore che verrà creato per il database SQL. Ci sono altri parametri che è possibile usare in un secondo momento.

![Finestra di PowerShell](automate-everything/_static/image2.png)

Al termine dello script, nel portale di gestione è possibile vedere cosa è stato creato. Sono disponibili due database:

![Database](automate-everything/_static/image3.png)

Un account di archiviazione:

![Account di archiviazione](automate-everything/_static/image4.png)

E un'app Web:

![Sito Web](automate-everything/_static/image5.png)

Nella scheda **Configura** dell'app Web è possibile vedere che le impostazioni dell'account di archiviazione e le stringhe di connessione al database SQL sono impostate per l'app Correggi it.

![appSettings e connectionStrings](automate-everything/_static/image6.png)

La cartella *automazione* contiene ora anche un file *&lt;websitename&gt;. pubxml* . In questo file vengono archiviate le impostazioni che MSBuild utilizzerà per distribuire l'applicazione nell'ambiente Azure appena creato. Esempio:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Come si può notare, lo script ha creato un ambiente di test completo e l'intero processo viene eseguito in circa 90 secondi.

Se un altro utente del team vuole creare un ambiente di test, può semplicemente eseguire lo script. Non solo è veloce, ma è anche possibile che si stia usando un ambiente identico a quello che si sta usando. Non è possibile essere certi che se tutti gli utenti impostassero manualmente gli elementi tramite l'interfaccia utente del portale di gestione.

### <a name="a-look-at-the-scripts"></a>Esaminare gli script

Sono effettivamente presenti tre script che eseguono questa operazione. È possibile chiamarne una dalla riga di comando e usa automaticamente le altre due per eseguire alcune delle attività:

- *New-AzureWebSiteEnv. ps1* è lo script principale.

    - *New-AzureStorage. ps1* crea l'account di archiviazione.
    - *New-AzureSql. ps1* crea i database.

### <a name="parameters-in-the-main-script"></a>Parametri nello script principale

Lo script principale, *New-AzureWebSiteEnv. ps1*, definisce diversi parametri:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Sono necessari due parametri:

- Nome dell'app Web creata dallo script. Viene usato anche per l'URL: `<name>.azurewebsites.net`.
- Password per il nuovo utente amministratore del server di database creato dallo script.

I parametri facoltativi consentono di specificare il percorso del data center (il valore predefinito è "Stati Uniti occidentali"), il nome dell'amministratore del server di database (il valore predefinito è "dbuser") e una regola del firewall per il server di database.

### <a name="create-the-web-app"></a>Creare l'app Web

La prima operazione eseguita dallo script consiste nel creare l'app Web chiamando il cmdlet `New-AzureWebsite`, passando al nome dell'app Web e ai valori dei parametri del percorso:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Creare l'account di archiviazione

Lo script principale esegue quindi lo script *New-AzureStorage. ps1* , specificando " *&lt;websitename&gt;* storage" per il nome dell'account di archiviazione e la stessa posizione data center dell'app Web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. ps1* chiama il cmdlet `New-AzureStorageAccount` per creare l'account di archiviazione e restituisce il nome dell'account e i valori della chiave di accesso. Per accedere ai BLOB e alle code nell'account di archiviazione, l'applicazione dovrà disporre di questi valori.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Potrebbe non essere sempre necessario creare un nuovo account di archiviazione. è possibile migliorare lo script aggiungendo un parametro che facoltativamente lo indirizza a usare un account di archiviazione esistente.

### <a name="create-the-databases"></a>Creare i database

Lo script principale esegue quindi lo script di creazione del database, *New-AzureSql. ps1*, dopo aver configurato i nomi predefiniti del database e del firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Lo script di creazione del database recupera l'indirizzo IP del computer di sviluppo e imposta una regola del firewall in modo che il computer di sviluppo possa connettersi al server e gestirlo. Lo script di creazione del database esegue quindi diversi passaggi per configurare i database:

- Crea il server utilizzando il cmdlet `New-AzureSqlDatabaseServer`.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crea regole del firewall per consentire al computer di sviluppo di gestire il server e di abilitare l'app Web per la connessione. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Consente di creare un contesto di database che include il nome del server e le credenziali utilizzando il cmdlet `New-AzureSqlDatabaseServerContext`.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` è una funzione nello script che chiama il cmdlet `ConvertTo-SecureString` per crittografare la password e restituisce un oggetto `PSCredential`, lo stesso tipo restituito dal cmdlet `Get-Credential`.
- Consente di creare il database dell'applicazione e il database di appartenenza utilizzando il cmdlet `New-AzureSqlDatabase`.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Chiama una funzione definita localmente per creare una stringa di connessione per ogni database. Per accedere ai database, l'applicazione utilizzerà tali stringhe di connessione. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString è una funzione definita nello script che crea la stringa di connessione dai valori di parametro forniti.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Restituisce una tabella hash con il nome del server di database e le stringhe di connessione.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

L'app Correggi it usa database di appartenenza e di applicazioni distinti. È anche possibile inserire sia l'appartenenza che i dati dell'applicazione in un singolo database.

### <a name="store-app-settings-and-connection-strings"></a>Archiviare le impostazioni dell'app e le stringhe di connessione

Azure dispone di una funzionalità che consente di archiviare le impostazioni e le stringhe di connessione che sostituiscono automaticamente ciò che viene restituito all'applicazione durante il tentativo di leggere le raccolte di `appSettings` o `connectionStrings` nel file Web. config. Si tratta di un'alternativa all'applicazione delle [trasformazioni di Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) durante la distribuzione di. Per altre informazioni, vedere [archiviare dati sensibili in Azure](source-control.md#appsettings) più avanti in questo e-book.

Lo script di creazione dell'ambiente archivia in Azure tutti i valori `appSettings` e `connectionStrings` necessari all'applicazione per accedere all'account di archiviazione e ai database quando viene eseguito in Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) è un Framework di telemetria illustrato nel capitolo [monitoraggio e telemetria](monitoring-and-telemetry.md) . Lo script di creazione dell'ambiente riavvia anche l'app Web per assicurarsi che rilevi le impostazioni di New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparazione per la distribuzione

Al termine del processo, lo script di creazione dell'ambiente chiama due funzioni per creare i file che verranno usati dallo script di distribuzione.

Una di queste funzioni crea un profilo di pubblicazione *(&lt;il file websitename&gt;. pubxml* ). Il codice chiama l'API REST di Azure per ottenere le impostazioni di pubblicazione e salva le informazioni in un file con *estensione publishsettings* . USA quindi le informazioni di tale file insieme a un file modello (*pubxml. template*) per creare il file con *estensione pubxml* che contiene il profilo di pubblicazione. Questo processo in due passaggi consente di simulare le operazioni eseguite in Visual Studio: scaricare un file con *estensione publishsettings* e importarlo per creare un profilo di pubblicazione.

L'altra funzione usa un altro file di modello (website-Environment. template) per creare un file *website-Environment. XML* che contiene le impostazioni che lo script di distribuzione userà insieme al file *pubxml* .

### <a name="troubleshooting-and-error-handling"></a>Risoluzione dei problemi e gestione degli errori

Gli script sono simili ai programmi: possono avere esito negativo e quando si vuole sapere quanto è possibile fare per l'errore e la causa. Per questo motivo, lo script di creazione dell'ambiente modifica il valore della variabile `VerbosePreference` da `SilentlyContinue` a `Continue` in modo che vengano visualizzati tutti i messaggi dettagliati. Modifica anche il valore della variabile `ErrorActionPreference` da `Continue` a `Stop`, in modo che lo script venga arrestato anche quando rileva errori non fatali:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Prima di eseguire qualsiasi operazione, lo script archivia l'ora di inizio in modo che sia in grado di calcolare il tempo trascorso:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Dopo aver completato il lavoro, lo script Visualizza il tempo trascorso:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

E per ogni operazione chiave, lo script scrive messaggi dettagliati, ad esempio:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script di distribuzione

Cosa fa lo script *New-AzureWebsiteEnv. ps1* per la creazione dell'ambiente, lo script *Publish-AzureWebsite. ps1* esegue la distribuzione dell'applicazione.

Lo script di distribuzione ottiene il nome dell'app Web dal file *website-Environment. XML* creato dallo script di creazione dell'ambiente.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Ottiene la password dell'utente di distribuzione dal file con *estensione publishsettings* :

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Esegue il comando [MSBuild](http://msbuildbook.com/) che compila e distribuisce il progetto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Se è stato specificato il parametro `Launch` nella riga di comando, viene chiamato il cmdlet `Show-AzureWebsite` per aprire il browser predefinito per l'URL del sito Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

È possibile eseguire lo script di distribuzione con un comando simile al seguente:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Al termine, il browser si apre con il sito in esecuzione nel cloud all'URL `<websitename>.azurewebsites.net`.

![Correggi l'app distribuita in Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Riepilogo

Con questi script è possibile essere certi che gli stessi passaggi verranno sempre eseguiti nello stesso ordine utilizzando le stesse opzioni. In questo modo si garantisce che ogni sviluppatore del team non perda qualcosa o non sia in grado di distribuire un elemento personalizzato nel proprio computer che non funziona in modo analogo nell'ambiente di un altro membro del team o nell'ambiente di produzione.

In modo analogo, è possibile automatizzare la maggior parte delle funzioni di gestione di Azure che è possibile eseguire nel portale di gestione, usando l'API REST, gli script di Windows PowerShell, un'API del linguaggio .NET o un'utilità bash che è possibile eseguire in Linux o Mac.

Nel [capitolo successivo](source-control.md) verrà esaminato il codice sorgente per spiegare perché è importante includere gli script nel repository del codice sorgente.

## <a name="resources"></a>Risorse

- [Installare e configurare Windows PowerShell per Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Viene illustrato come installare i cmdlet di Azure PowerShell e come installare il certificato necessario nel computer per gestire l'account Azure. Questo è un ottimo punto di partenza perché contiene anche collegamenti a risorse per l'apprendimento di PowerShell.
- [Centro per script di Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portale di WindowsAzure.com per le risorse per lo sviluppo di script per la gestione dei servizi di Azure, con collegamenti a esercitazioni introduttive, documentazione di riferimento per i cmdlet e codice sorgente e script di esempio
- [Scripter del weekend: introduzione con Azure e PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In un Blog dedicato a Windows PowerShell, questo post fornisce un'ottima introduzione all'uso di PowerShell per le funzioni di gestione di Azure.
- [Installare e configurare l'interfaccia della riga di comando multipiattaforma di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Esercitazione introduttiva per un Framework di scripting di Azure che funziona su Mac e Linux, nonché sui sistemi Windows.
- [Sezione strumenti della riga di comando dell'argomento scaricare gli SDK e gli strumenti di Azure](https://azure.microsoft.com/downloads/). Pagina del portale per la documentazione e i download relativi agli strumenti da riga di comando per Azure.
- Post del blog di Scott Hanselman sull'[automazione completa con le librerie di gestione di Azure e .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselt introduce l'API di gestione .NET per Azure.
- [Utilizzo degli script di Windows PowerShell per la pubblicazione in ambienti di sviluppo e test](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentazione MSDN in cui viene illustrato come utilizzare gli script di pubblicazione generati automaticamente da Visual Studio per i progetti Web.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Estensione di Visual Studio che aggiunge il supporto per la lingua per Windows PowerShell in Visual Studio.

> [!div class="step-by-step"]
> [Precedente](introduction.md)
> [Successivo](source-control.md)
