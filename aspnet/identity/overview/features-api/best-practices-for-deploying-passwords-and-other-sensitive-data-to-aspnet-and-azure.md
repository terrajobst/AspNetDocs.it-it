---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Distribuzione di password e altri dati sensibili in ASP.NET e app Azure Service-ASP.NET 4. x
author: Rick-Anderson
description: Questa esercitazione illustra come il codice può archiviare e accedere in modo sicuro alle informazioni protette. Il punto più importante è non archiviare mai le password o altri Sen...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584322"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Procedure consigliate per la distribuzione delle password e di altri dati sensibili in ASP.NET e in Servizio app di Azure

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra come il codice può archiviare e accedere in modo sicuro alle informazioni protette. Il punto più importante è non archiviare mai le password o altri dati sensibili nel codice sorgente e non usare i segreti di produzione in modalità di sviluppo e test.
> 
> Il codice di esempio è una semplice app console processo Web e un'app ASP.NET MVC che richiede l'accesso a una stringa di connessione di database password, Twilio, Google e SendGrid chiavi sicure.
> 
> Sono citate anche le impostazioni locali e PHP.

- [Utilizzo delle password nell'ambiente di sviluppo](#pwd)
- [Utilizzo delle stringhe di connessione nell'ambiente di sviluppo](#con)
- [App console processi Web](#wj)
- [Distribuzione di segreti in Azure](#da)
- [Note per l'ambiente locale e PHP](#not)
- [Risorse aggiuntive](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Utilizzo delle password nell'ambiente di sviluppo

Le esercitazioni mostrano spesso dati sensibili nel codice sorgente, con la speranza che non vengano mai archiviati dati sensibili nel codice sorgente. Ad esempio, l'esercitazione sull' [app ASP.NET MVC 5 con SMS e posta elettronica 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Mostra quanto segue nel file *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Il file *Web. config* è codice sorgente, quindi questi segreti non devono mai essere archiviati in tale file. Fortunatamente, l'elemento `<appSettings>` dispone di un attributo `file` che consente di specificare un file esterno che contiene le impostazioni di configurazione dell'app riservate. È possibile spostare tutti i segreti in un file esterno, purché il file esterno non venga archiviato nell'albero di origine. Nel markup seguente, ad esempio, il file *AppSettingsSecrets. config* contiene tutti i segreti dell'app:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Il markup nel file esterno (*AppSettingsSecrets. config* in questo esempio) corrisponde al markup trovato nel file *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Il runtime di ASP.NET unisce il contenuto del file esterno con il markup nell'elemento &lt;appSettings&gt;. Il runtime ignora l'attributo del file, se non è possibile trovare il file specificato.

> [!WARNING]
> Sicurezza: non aggiungere il file *Secrets. config* al progetto o archiviarlo nel controllo del codice sorgente. Per impostazione predefinita, Visual Studio imposta il `Build Action` su `Content`, il che significa che il file viene distribuito. Per altre informazioni, vedere [perché non vengono distribuiti tutti i file nella cartella del progetto?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Sebbene sia possibile usare qualsiasi estensione per il file *Secrets. config* , è preferibile mantenerla *. config*, perché i file di configurazione non vengono serviti da IIS. Si noti inoltre che il file *AppSettingsSecrets. config* è costituito da due livelli di directory fino al file *Web. config* , quindi è completamente fuori dalla directory della soluzione. Spostando il file fuori dalla directory della soluzione, &quot;git Aggiungi \*&quot; non lo aggiungerà al repository.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Utilizzo delle stringhe di connessione nell'ambiente di sviluppo

Visual Studio crea nuovi progetti ASP.NET che usano il [database locale](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Il database locale è stato creato in modo specifico per l'ambiente di sviluppo. Non richiede una password, pertanto non è necessario eseguire alcuna operazione per impedire che i segreti vengano archiviati nel codice sorgente. Alcuni team di sviluppo utilizzano le versioni complete di SQL Server (o di altri sistemi DBMS) che richiedono una password.

Per sostituire l'intero markup di `<connectionStrings>`, è possibile usare l'attributo `configSource`. A differenza dell'attributo `<appSettings>` `file` che unisce il markup, l'attributo `configSource` sostituisce il markup. Il markup seguente mostra l'attributo `configSource` nel file *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Se si usa l'attributo `configSource`, come illustrato in precedenza, per spostare le stringhe di connessione in un file esterno e fare in modo che Visual Studio crei un nuovo sito Web, non sarà in grado di rilevare che si sta usando un database e non si avrà la possibilità di configurare il database durante la pubblicazione in Azure da Visual Studio. Se si usa l'attributo `configSource`, è possibile usare PowerShell per creare e distribuire il sito Web e il database oppure è possibile creare il sito Web e il database nel portale prima di pubblicare. Lo script [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) creerà un nuovo sito Web e un nuovo database.

> [!WARNING]
> Sicurezza: diversamente dal file *AppSettingsSecrets. config* , il file di stringhe di connessione esterno deve trovarsi nella stessa directory del file *Web. config* radice, quindi è necessario adottare le precauzioni per assicurarsi di non archiviarlo nel repository di origine.

> [!NOTE]
> **Avviso di sicurezza per il file Secrets:** Una procedura consigliata consiste nel non usare i segreti di produzione per test e sviluppo. L'uso di password di produzione in test o sviluppo perde tali segreti.

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>App console processi Web

Il file *app. config* usato da un'app console non supporta percorsi relativi, ma supporta percorsi assoluti. È possibile usare un percorso assoluto per spostare i segreti dalla directory del progetto. Il markup seguente mostra i segreti nel file *C:\secrets\AppSettingsSecrets.config* e i dati non sensibili nel file *app. config* .

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Distribuzione di segreti in Azure

Quando si distribuisce l'app Web in Azure, il file *AppSettingsSecrets. config* non verrà distribuito (questo è quello che si vuole). È possibile passare al [portale di gestione di Azure](https://azure.microsoft.com/services/management-portal/) e impostarli manualmente, a tale scopo:

1. Passare a [https://portal.azure.com](https://portal.azure.com)e accedere con le credenziali di Azure.
2. Fare clic su **sfoglia &gt; app Web**, quindi fare clic sul nome dell'app Web.
3. Fare clic su **tutte le impostazioni &gt; impostazioni dell'applicazione**.

Le **impostazioni dell'app** e i valori della **stringa di connessione** sostituiscono le stesse impostazioni nel file *Web. config* . In questo esempio le impostazioni non sono state distribuite in Azure, ma se queste chiavi si trovavano nel file *Web. config* , le impostazioni visualizzate nel portale avrebbero la precedenza.

Una procedura consigliata consiste nel seguire un [flusso di lavoro di DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) e usare [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (o un altro Framework, ad esempio [chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) per automatizzare l'impostazione di questi valori in Azure. Il seguente script di PowerShell usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) per esportare i segreti crittografati su disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Nello script precedente,' name ' è il nome della chiave privata, ad esempio '&quot;FB\_AppSecret&quot; o "TwitterSecret". È possibile visualizzare il file ". Credential" creato dallo script nel browser. Il frammento di codice seguente verifica ogni file di credenziali e imposta i segreti per l'app Web denominata:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sicurezza: non includere password o altri segreti nello script di PowerShell. in questo modo si vanifica lo scopo dell'uso di uno script di PowerShell per distribuire dati riservati. Il cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) fornisce un meccanismo sicuro per ottenere una password. L'uso di un prompt dell'interfaccia utente può impedire la perdita di password.

### <a name="deploying-db-connection-strings"></a>Distribuzione delle stringhe di connessione del database

Le stringhe di connessione del database vengono gestite in modo analogo alle impostazioni dell'app. Se si distribuisce l'app Web da Visual Studio, la stringa di connessione viene configurata per l'utente. È possibile verificarlo nel portale. Il metodo consigliato per impostare la stringa di connessione è con PowerShell. Per un esempio di script di PowerShell, viene creato un sito Web e un database e viene impostata la stringa di connessione nel sito Web, scaricare [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dalla [libreria di script di Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Note per PHP

Poiché le coppie chiave-valore per **le impostazioni delle app** e le **stringhe di connessione** vengono archiviate in variabili di ambiente nel servizio app Azure, gli sviluppatori che usano qualsiasi framework di app Web (ad esempio php) possono recuperare facilmente questi valori. Vedere il post di Blog relativo al funzionamento delle [stringhe di connessione e](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) delle stringhe di connessione di Stefan Schackow, che mostra un frammento di codice php per leggere le impostazioni dell'app e le stringhe di connessione.

## <a name="notes-for-on-premises-servers"></a>Note per i server locali

Se si esegue la distribuzione in server Web locali, è possibile proteggere [i segreti crittografando le sezioni di configurazione dei file di configurazione](https://msdn.microsoft.com/library/ff647398.aspx). In alternativa, è possibile usare lo stesso approccio consigliato per siti Web di Azure: Mantieni le impostazioni di sviluppo nei file di configurazione e usa i valori delle variabili di ambiente per le impostazioni di produzione. In questo caso, tuttavia, è necessario scrivere il codice dell'applicazione per le funzionalità automatiche in siti Web di Azure: recuperare le impostazioni dalle variabili di ambiente e usare tali valori al posto delle impostazioni del file di configurazione oppure usare le impostazioni del file di configurazione quando le variabili di ambiente non sono state trovate.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

Per un esempio di uno script di PowerShell che crea un'app Web e un database, imposta la stringa di connessione e le impostazioni dell'app, scaricare [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dalla [libreria di script di Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Vedere siti Web di Windows Azure di Stefan Schackow: funzionamento delle [stringhe di applicazione e delle stringhe di connessione](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

Grazie speciale a Barry Dorrans ( [@blowdart](https://twitter.com/blowdart) ) e Carlos Farre per la revisione.
