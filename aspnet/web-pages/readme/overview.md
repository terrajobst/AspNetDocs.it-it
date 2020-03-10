---
uid: web-pages/readme/overview
title: Leggimi di WebMatrix | Microsoft Docs
author: rick-anderson
description: File Leggimi della versione di WebMatrix e Pagine Web ASP.NET (Razor) 1,0
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563469"
---
# <a name="webmatrix-readme"></a>File Leggimi di WebMatrix

13 gennaio 2011

## <a name="contents"></a>Contenuto

> [!NOTE]
> Questo file Leggimi si applica alla versione 1,0 di WebMatrix.

- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Come pubblicare le applicazioni](#InstructionsForPublishingApplications)
- [Modifiche e problemi](#ChangesAndIssues)

    - [Installazione di WebMatrix 1,0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
- [Ulteriori informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix 1,0 è uno stack di sviluppo web gratuito che viene installato in pochi minuti. Integra un server Web con Framework di programmazione e database per creare un'unica esperienza integrata. È possibile usare WebMatrix per semplificare il codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile usare WebMatrix per avviare un nuovo sito Web usando le app open source più diffuse, ad esempio DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix usa lo stesso ambiente di server Web, motore di database e Framework che eseguirà il tuo sito Web su Internet, il che rende la transizione dallo sviluppo alla produzione senza problemi.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix 1,0, è necessario prima installare il [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarla per installare WebMatrix.
> 
> Se si verificano problemi durante l'installazione, vedere [risoluzione dei problemi con installazione guidata piattaforma Web Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Come pubblicare le applicazioni

> Vedere [le istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Modifiche e problemi

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemi di installazione di WebMatrix 1,0

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1,0 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4

> Per WebMatrix è necessario il .NET Framework versione 4. In alcuni casi, il programma di installazione di WebMatrix 1,0 consente di provare a eseguire l'installazione in una piattaforma che non fa parte del set di configurazione supportato. In particolare, Windows Vista senza aggiornamento SP1 consentirà di iniziare l'installazione di WebMatrix, ma il componente .NET Framework 4 avrà esito negativo e bloccherà l'installazione.
> 
> **Soluzione alternativa**  
> Installare in una piattaforma supportata, che include:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 o versione successiva
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: non è possibile installare WebMatrix 1,0 se Microsoft Visual Studio 2008 è installato senza Microsoft Visual Studio 2008 SP1

> **Soluzione alternativa**  
> Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: alcuni assembly per SQL Server Compact 4,0 non sono installati nella GAC

> Gli assembly gestiti per SQL Server Compact 4,0 non vengono inseriti nella Global Assembly Cache (GAC) quando si installa SQL Server Compact 4,0 in un computer a 64 bit e il computer dispone solo del profilo client .NET Framework 3,5 SP1 installato. Gli assembly gestiti che non sono installati nella GAC sono:
> 
> - *System. Data. SqlServerCe. dll* (provider ADO.NET)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4,0. Scaricare e installare la versione completa di .NET Framework 3,5 SP1 dal percorso seguente:  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Reinstallare SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: Impossibile disinstallare SQL Server Compact tramite la riga di comando

> La disinstallazione di SQL Server Compact mediante opzioni della riga di comando non funziona in questa versione.
> 
> **Soluzione alternativa**  
> Usare *programmi e funzionalità* nel pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4,0.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e i problemi noti relativi alla versione 1,0 di Pagine Web ASP.NET con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

#### <a id="NewFeatures"></a>Nuove funzionalità

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nuovo: impostazione di configurazione aggiunta per disabilitare Gestione pacchetti

> È disponibile una nuova chiave di `asp:AdminManagerEnabled` per l'elemento `<appSettings>` nel file *Web. config* , che consente di disabilitare completamente gestione pacchetti. Il valore predefinito per questo elemento è true, ovvero se non è incluso nel file *Web. config* , gestione pacchetti è abilitato. Per disabilitare Gestione pacchetti, aggiungere il seguente elemento al file *Web. config* nella radice del sito Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>Modifiche

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Modifica: "pagine Web: AdminFolderVirtualPath" la chiave è stata rinominata "ASP: AdminFolderVirtualPath"

> La chiave di `webPages:AdminFolderVirtualPath` che può essere aggiunta al file *Web. config* per specificare il percorso di gestione pacchetti è stata rinominata in modo da utilizzare lo spazio dei nomi `asp:` invece dello spazio dei nomi `webPages`. Se è stato usato questo elemento, è necessario rinominarlo nel file di configurazione.

#### <a id="Issues"></a> Problemi noti

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: le password per gli utenti di appartenenza non sono più riconosciute

> L'algoritmo per la creazione e l'archiviazione delle password di appartenenza (accesso) è stato modificato in modo da essere più sicuro. Di conseguenza, le password archiviate per i membri (utenti) creati nelle versioni beta di ASP.NET Razor non verranno riconosciute. 
> 
> **Soluzione alternativa** Se il sito non è ancora stato inserito in produzione, rimuovere i record utente dal database delle appartenenze. Se il database è attivo, rigenera le password esistenti a livello di codice nel database delle appartenenze.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamento imprevisto quando si usa una tabella utente personalizzata per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web Razor di ASP.NET, chiamare il metodo `WebSecurity.InitializeDatabaseConnection`. (In WebMatrix, il modello di sito Starter include una chiamata a questo metodo nel file *\_AppStart. cshtml* ). Se il parametro `autoCreateTables` di questo metodo è impostato su true (per impostazione predefinita, è impostato su true nel modello di sito Starter) e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera alcun errore. Viene invece creata automaticamente la tabella.
> 
> Questo può costituire un problema se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma si passa il nome di tabella errato al metodo `WebSecurity.InitializeDatabaseConnection`. Poiché per impostazione predefinita il metodo non genera un errore se la tabella specificata non esiste e viene invece creata una nuova tabella, l'applicazione può sembrare funzionante. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui relativi campi) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Verificare che il nome passato nel metodo `InitializeDatabaseConnection` corrisponda alla tabella dei profili utente nel database delle appartenenze o verificare che il parametro `autoCreateTables` sia impostato su false.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problema: messaggio di errore "il modulo di amministrazione richiede l'accesso a ~/app\_dati"

> In alcune circostanze, se si tenta di creare utenti o di lavorare con il sistema di appartenenze di ASP.NET, è possibile che nella pagina venga visualizzato l'errore *il modulo di amministrazione richiede l'accesso a ~/App\_dati*. Questo errore si verifica se l'account con cui è in esecuzione IIS o IIS Express non dispone delle autorizzazioni per creare e scrivere nella cartella *App\_data* nella radice del sito Web. 
> 
> **Soluzione alternativa** Creare manualmente una cartella *App\_data* per il sito Web. Assicurarsi quindi che l'account di Windows con cui viene eseguita l'applicazione (in genere servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio i dati\_app. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge base [problemi relativi a SQL Server Express istanze utente e ai progetti di applicazione Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "Impossibile generare un'istanza utente di SQL Server" errore

> Se un'applicazione Web WebMatrix usa SQL Server Express ed esegue IIS 7,5 in Windows 7 o Windows Server 2008 R2, è possibile che venga visualizzato un errore che indica che SQL Server non è in grado di recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** Verificare che l'account di Windows con cui viene eseguita l'applicazione (in genere servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *i dati\_app*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge base [problemi relativi a SQL Server Express istanze utente e ai progetti di applicazione Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: i file che contengono le risorse di gestione pacchetti o le password di gestione pacchetti sono utilizzabili in IIS 6,0 e versioni precedenti

> Se si distribuisce un'applicazione Pagine Web ASP.NET (Razor) compilata con la versione RC2 e se l'applicazione contiene un file *password. txt* o *PackageSources. txt* in */app\_Data/admin*, IIS 6,0 servirà il file, se necessario, esponendo potenzialmente le password per l'istanza di gestione pacchetti. 
> 
> **Soluzione alternativa** Rinominare il file *password. txt* o *PackageSources. txt* in *password. config* o *PackageSources. config*. Per impostazione predefinita, IIS 6,0 non gestisce i file con estensione *config* . (In IIS 7, non sono serviti file nell' *App\_cartella dati* , pertanto non è necessario rinominare i file).

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: la disinstallazione dei pacchetti installati con la versione beta 3 non rimuove completamente i componenti del pacchetto

> Se è stato installato un pacchetto usando Gestione pacchetti nella versione beta 3 e quindi si prova a disinstallarlo usando la versione corrente, il pacchetto non è completamente disinstallato. L'utilizzo del pulsante di **disinstallazione** di gestione pacchetti consente di rimuovere alcuni componenti, lasciando il codice di libreria del pacchetto e non aggiorna il file *Package. config* .
> 
> **Soluzione alternativa**   
> Eseguire questi passaggi:  
> 1. Eliminare l' *App\_cartella Data\packages* . Verranno rimossi tutti i pacchetti.   
> 2. Eliminare il file *packages. config* nella directory principale del sito Web.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: in Visual Studio la chiamata di gestione pacchetti basata sul Web porta l'applicazione offline

> Se si lavora in Visual Studio (non WebMatrix) e si usa la funzionalità di *amministrazione di\_* per avviare Gestione pacchetti, Visual Studio porta l'applicazione offline e invia l' *app\_offline. htm* nella radice del sito Web, che interrompe la possibilità di usare Gestione pacchetti.
> 
> [!NOTE]
> Sebbene in genere questo comportamento venga visualizzato quando si usa l'interfaccia di gestione pacchetti basata sul Web, lo stesso comportamento si verifica se si aggiungono, rimuovono o si modificano i file nella cartella *App\_data* .
> 
> **Soluzione alternativa**   
> Per lavorare con i pacchetti in Visual Studio, usare l'estensione NuGet invece di gestione pacchetti basata sul Web. Per informazioni, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/). Se si lavora con altri file nella cartella *App\_data* , è consigliabile mantenere i file altrove per evitare questo problema. Se questo non è pratico, eliminare manualmente l' *app\_file offline. htm* o attendere che il sito torni automaticamente online (per impostazione predefinita, dopo 30 secondi).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: i modelli di progetto e IntelliSense di Visual Studio sono disponibili solo in ASP.NET MVC versione 3

> L'installazione di Pagine Web ASP.NET non installa anche strumenti per Visual Studio, ad esempio IntelliSense e modelli di progetto per le applicazioni Pagine Web ASP.NET.
> 
> **Soluzione alternativa** Per usare IntelliSense e i modelli di progetto per applicazioni Pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: lettura di feed o altri dati esterni tramite un server proxy

> Se il server in cui è in esecuzione il sito si trova dietro un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel file *Web. config* per poter leggere le informazioni che provengono dall'esterno del sito. Se ad esempio si usa l'helper `ReCaptcha`, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy. Analogamente, i feed usati in Pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti, potrebbero richiedere la configurazione del proxy.
> 
> Se si verificano problemi durante l'uso di un servizio esterno o l'uso del feed di pacchetti, inserire gli elementi seguenti nel file *Web. config* radice dell'applicazione:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Per ulteriori informazioni sulla configurazione di un server proxy, vedere [&lt;elemento proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) nel sito Web MSDN.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: la disinstallazione di .NET Framework versione 4 Disabilita Pagine Web ASP.NET con la sintassi Razor

> Se si disinstalla il .NET Framework versione 4 e quindi lo si reinstalla, Pagine Web ASP.NET con sintassi Razor è disabilitato. Le pagine con estensione *cshtml* non vengono eseguite correttamente. Pagine Web ASP.NET registra un assembly nel file *Web. config* radice del computer e rimuovendo il .NET Framework rimuove il file. Con la reinstallazione del .NET Framework viene installata una nuova versione del file di configurazione, ma non viene aggiunto il riferimento per l'assembly di Pagine Web ASP.NET.
> 
> **Soluzione alternativa** Dopo aver reinstallato il .NET Framework, reinstallare Pagine Web ASP.NET con sintassi Razor. In questo modo viene aggiunto il seguente elemento al file *Web. config* nella radice del computer, che in genere si trova nel percorso seguente:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: gli URL senza estensione non trovano i file con estensione cshtml/vbhtml in IIS 7 o IIS 7,5

> In IIS 7 o IIS 7,5, le richieste con un URL simile al seguente non sono in grado di trovare le pagine con estensione *cshtml* o *vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Il problema si verifica perché la riscrittura degli URL non è abilitata per impostazione predefinita per IIS 7 o IIS 7,5. Lo scenario probabile è che il problema non viene visualizzato durante i test in locale usando IIS Express, ma si verifica quando si distribuisce il sito Web in un sito Web di hosting.
> 
> **Soluzione alternativa**
> 
> - Se si ha il controllo sul computer server, nel computer server installare l'aggiornamento descritto in [un aggiornamento è disponibile che consente a determinati gestori iis 7,0 o iis 7,5 di gestire le richieste i cui URL non terminano con un punto](https://support.microsoft.com/kb/980368).
> - Se non si ha il controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere quanto segue al file *Web. config* del sito Web: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: distribuzione di un'applicazione in un computer in cui non è installato SQL Server Compact

> Le applicazioni che includono SQL Server Compact database possono essere eseguite in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix 1,0 copia automaticamente questi file binari ed esegue le trasformazioni appropriate del file *Web. config* .
> 
> **Soluzione alternativa** Se è necessario copiare questi file e modificare manualmente il file *Web. config* , eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database nella cartella *bin* (e nelle sottocartelle) dell'applicazione nel computer di destinazione:  
> 
>    - Copia *c:\programmi\microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **a** *\bin*
>    - Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **in** *\Bin\x86*
>    - Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **in** *\Bin\amd64*
> 
> 2. Nella cartella radice del sito Web creare o aprire un file *Web. config* . (In WebMatrix 1,0, questo tipo di file è disponibile se si fa clic su **tutto** nella finestra di dialogo **scegliere un tipo di file** ).
> 3. Aggiungere l'elemento seguente come figlio dell'elemento `<configuration>` (non all'interno dell'elemento `<system.web>`):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: gli helper "database" e "WebGrid" non funzionano con attendibilità media in Visual Basic

> Se si usa Visual Basic (creazione di file con *estensione vbhtml* ), gli helper `Database` e `WebGrid` non funzioneranno se l'applicazione è impostata per usare l'attendibilità media.
> 
> **Soluzione alternativa**  
> Se si utilizza Visual Studio 2010, è possibile risolvere il problema installando la versione di Service Pack 1. Fino a quando non è disponibile la versione finale di SP1, è possibile scaricare la versione beta di SP1 dalla pagina [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) nell'area download Microsoft.   
>   
> In caso contrario, se non si utilizza Visual Studio 2010, è possibile impostare temporaneamente l'applicazione per l'utilizzo dell'attendibilità totale.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: le risorse "ApplicationPart" sono accessibili dall'esterno

> Se un assembly contiene oggetti che derivano dalla classe `ApplicationPart`, le risorse dell'assembly vengono esposte dalla classe `ResourceRouteHandler`. Si consideri ad esempio l'URL seguente:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Questa richiesta Scarica tutte le stringhe di risorsa nell'assembly *System. Web. WebPages. Administration. dll* . Vengono scaricate tutte le risorse incorporate (anche quelle che non sono progettate per essere gestite come contenuto statico). Se le risorse incorporate contengono informazioni riservate, questo può rappresentare un rischio per la sicurezza. 
> 
> **Soluzione alternativa**   
> Se si crea un oggetto **ApplicationPart** , verificare che le risorse incorporate associate all'assembly dell'oggetto **ApplicationPart** non contengano informazioni riservate.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Per informazioni sui problemi di installazione per WebMatrix, vedere la pagina relativa ai [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.

Questa sezione del documento descrive i problemi noti per l'ambiente di sviluppo WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: le modifiche apportate al nome utente o alla password di una stringa di connessione al database in un file Web. config non vengono riflesse nell'area di lavoro database

> **Soluzione alternativa**  
> 
> 1. Nel file *Web. config* , modificare il nome del database nella stringa di connessione (ad esempio, aggiungere "1").
> 2. Salvare il file *Web. config* .
> 3. Fare clic su **database** e quindi su Aggiorna.
> 4. Modificare il nome del database nella stringa di connessione nel file *Web. config* con il nome del database originale.
> 5. Salvare il file *Web. config* .
> 6. Fare clic su **database** e quindi su Aggiorna.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: non è possibile eliminare le cartelle create da WebMatrix

> Se WebMatrix viene eseguito usando autorizzazioni elevate, ovvero è stato avviato WebMatrix usando l'opzione **Esegui come amministratore** in Windows, le cartelle create da WebMatrix non possono essere eliminate tramite Esplora risorse.
> 
> **Soluzione alternativa**  
> Eseguire Esplora risorse con autorizzazioni elevate. Attenersi ai passaggi riportati di seguito.  
> 
> 1. In Windows fare clic su **Avvia**.
> 2. Immettere "Esplora risorse" e fare clic con il pulsante destro del mouse sulla voce di **Esplora risorse**.
> 3. Fare clic su **Esegui come amministratore**. È quindi possibile eliminare le cartelle.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1,0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi

> WebMatrix 1,0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7 si è connessi con un account che non dispone di privilegi amministrativi e il controllo dell'account utente è disabilitato.
> - Si sta utilizzando Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività in WebMatrix 1,0 non richiede autorizzazioni amministrative. Per tali operazioni, è possibile eseguire l'operazione come amministratore o attenersi alla procedura seguente:
> 
> - In Windows Vista o Windows 7 abilitare UAC.
> - In Windows XP aggiungere l'utente al gruppo di sicurezza Administrators.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "sito da raccolta web" disabilitato

> Se l'installazione guidata piattaforma Web 3,0 non è installata, l'opzione **sito da raccolta web** è disabilitata.
> 
> **Soluzione alternativa**  
> Installare il [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome non è disponibile come opzione di esecuzione

> Google Chrome non viene visualizzato nell'elenco dei browser in **Esegui** nella scheda **Home** .
> 
> **Soluzione alternativa**  
> Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti di Windows. Come soluzione alternativa, avviare Google Chrome, fare clic sul menu *Personalizza e controlla Google Chrome* , scegliere *Opzioni*e quindi fare clic su *crea Google Chrome come browser predefinito*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: la finestra di dialogo "chiave esterna" non consente l'immissione di una chiave primaria

> La finestra di dialogo **chiave esterna** non consente di immettere il nome della chiave primaria dalla tabella della chiave primaria.
> 
> **Soluzione alternativa**  
> Questo comportamento è intenzionale. Non è necessario immettere il nome della chiave primaria dalla tabella della chiave primaria.

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense non è disponibile in WebMatrix per sintassi Razor, C#o Visual Basic

> IntelliSense è supportato in WebMatrix per HTML e CSS. Tuttavia, non è disponibile per altre lingue. 
> 
> **Soluzione alternativa**   
> Nessuna.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense per HTML e CSS suggerisce elementi che non sono contestualemente appropriati

> IntelliSense per markup in WebMatrix supporta HTML usando lo schema di [transizione XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e CSS usando lo [schema CSS 2,1](http://www.w3.org/TR/CSS2/). Poiché IntelliSense è basato su questi schemi specifici, è possibile suggerire alcuni tag, attributi o proprietà che non sono appropriati per la definizione di pagina o di stile corrente. Per HTML, può anche causare suggerimenti imprevisti nel contenuto che potrebbero essere interpretati come XHTML non valido (ad esempio, quando i tag non sono chiusi). Questo problema potrebbe essere più evidente se il punto di inserimento si trova all'interno di un tag incompleto; in tal caso, IntelliSense potrebbe suggerire nuovi tag di apertura o offrire altri suggerimenti non corretti. 
> 
> **Soluzione alternativa**   
> Per HTML, assicurarsi di lavorare all'interno di una pagina XHTML ben formata e completa. Per CSS non esiste alcuna soluzione alternativa.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: IntelliSense non viene richiamato durante la digitazione

> In alcuni casi, IntelliSense potrebbe non essere richiamato come HTML o CSS immesso nell'editor. In particolare, questa situazione può verificarsi quando il punto di inserimento si trova direttamente accanto a un altro elemento o alla fine di un file. 
> 
> **Soluzione alternativa**   
> Verificare che sia presente uno spazio vuoto intorno al punto di inserimento e che il punto di inserimento non si trovi alla fine di un file. È anche possibile richiamare IntelliSense manualmente premendo CTRL + barra spaziatrice.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: non è disponibile alcuna interfaccia utente per la disabilitazione di IntelliSense

> WebMatrix 1,0 non fornisce un'interfaccia utente o un movimento per la disabilitazione di IntelliSense. 
> 
> **Soluzione alternativa**   
> Avviare WebMatrix usando il comando seguente, che include un'opzione che Disabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express dispone di un proprio file Leggimi, disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact dispone di un proprio file Leggimi, disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Per informazioni sui problemi che comportano l'installazione di SQL Server Compact come parte di WebMatrix, vedere la pagina relativa ai [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.

### <a id="Known_Issues_Installing_Applications"></a>Installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: l'installazione di un'applicazione può richiedere molto tempo se la cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuna. L'installazione dell'applicazione potrebbe richiedere un po' di tempo, ma verrà installata correttamente.

### <a id="Known_Issues_Publishing_Applications"></a>Pubblicazione di applicazioni

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "Impossibile acquisire le autorizzazioni necessarie" durante la pubblicazione di un database SQL Compact

> WebMatrix non supporta completamente la distribuzione di file binari di supporto per SQL Server Compact a un server che esegue .NET Framework versione 3,5 con una configurazione con attendibilità media.
> 
> **Soluzione alternativa**  
> La soluzione alternativa preferita consiste nell'installare il .NET Framework 4 sul server. In alternativa, eseguire le operazioni seguenti:
> 
> 1. Aggiungere gli elementi seguenti alla sezione `SecurityClasses` nel file *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Creare un nuovo set di autorizzazioni nel file *Web\_MediumTrust. config* con le autorizzazioni necessarie seguenti:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Applicare il set di autorizzazioni a SQL Server Compact inserendo gli elementi seguenti nel file *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: la raccolta e le applicazioni Web PhpBB visualizzano un errore "servizio non disponibile" dopo la pubblicazione

> In alcune circostanze, la pubblicazione di un'applicazione causa un errore di "servizio non disponibile".
> 
> **Soluzione alternativa**  
> In WebMatrix aggiungere una barra rovesciata (\) alla fine del nome del server nella finestra **impostazioni di pubblicazione** e quindi pubblicare di nuovo l'applicazione.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: il layout del sito web Moodle e i collegamenti vengono interrotti dopo la pubblicazione

> Dopo la pubblicazione di un'applicazione Moodle, l'applicazione non funzionerà correttamente.
> 
> **Soluzione alternativa**  
> In WebMatrix aggiungere una barra (/) alla fine del campo **nome sito** nella finestra **impostazioni di pubblicazione** e quindi pubblicare di nuovo l'applicazione.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: la pubblicazione del nopCommerce ha esito negativo con un errore di database

> La pubblicazione di nopCommerce ha esito negativo e segnala un errore di database come "Insert into the NOP\_log table failed".
> 
> **Soluzione alternativa**  
> 
> 1. In WebMatrix fare clic su **Esegui** per avviare nopCommerce localmente.
> 2. Accedere alla pagina di amministrazione.
> 3. Scegliere il menu **sistema** .
> 4. Fare clic sull'opzione **log** .
> 5. Fare clic sul pulsante **Cancella registro**.
> 6. Pubblicare nuovamente nopCommerce.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: SilverStripe CMS Visualizza un errore "HTTP 500 PHP FCGI" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Dopo aver fatto clic su **Scarica sito pubblicato**, ignorare `silverstripe-cache/manifest_main` nell' **Anteprima di pubblicazione**. Questo file viene usato per la memorizzazione nella cache ed è specifico per ogni computer.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: nel sottotesto viene visualizzato il messaggio "errore del server nell'applicazione '/'" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Aprire il file *Web. config* del sito e sostituire l'ID utente e la password nella stringa di connessione al database con le credenziali di amministratore SQL Server (le credenziali "sa").
> 
> In alternativa, attenersi alla seguente procedura per assegnare all'account utente che si è connessi con `db_owner` autorizzazioni:
> 
> 1. Installare SQL Server Management Studio usando l'installazione guidata piattaforma Web.
> 2. Connettersi all'istanza di SQL Server Express locale (per impostazione predefinita, `.\SQLEXPRESS`).
> 3. Fare clic su **database** &gt; *[LocalSubtextDatabase]* &gt; **sicurezza** &gt; **utenti** &gt; *[localSubtextUser*] (impostazione predefinita: `subtextuser`], fare clic con il pulsante destro del mouse e scegliere **Proprietà**.
> 4. Selezionare **database\_Owner** nella sezione appartenenza a ruoli.

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: il sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è preceduto da http://o https://

> Nella finestra di dialogo **impostazioni di pubblicazione** , se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.
> 
> **Soluzione alternativa**  
> Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nella finestra di dialogo **impostazioni di pubblicazione** inizi con `http://` o `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: la pubblicazione di un database MySQL ha esito negativo con l'errore "Impossibile pubblicare il database. Questo problema può verificarsi se il database remoto non è in grado di eseguire lo script ".

> L'errore può verificarsi per diversi motivi. Uno dei motivi per cui è possibile visualizzare questo errore è che lo script del database contiene una virgoletta singola (') e il set di caratteri predefinito del database MySQL di destinazione non è UTF-8.
> 
> **Soluzione alternativa**  
> Impostare il set di caratteri predefinito per il database MySQL remoto su UTF-8.

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: alcuni collegamenti non sono visibili in DotNetNuke dopo la pubblicazione o il download del sito

> Se si pubblica o si scarica un sito DotNetNuke, potrebbe essere necessario cancellare la cache per visualizzare i nuovi collegamenti da visualizzare nel sito.
> 
> **Soluzione alternativa**
> 
> 1. Accedere come "host".
> 2. Passare al menu host e selezionare **Impostazioni host**.
> 3. Scorrere verso il basso e in **Impostazioni avanzate**, espandere **Impostazioni prestazioni**.
> 4. Fare clic sul collegamento **Cancella cache** per le pagine.
> 5. Passare alla parte inferiore della pagina e riavviare l'applicazione.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: alcuni collegamenti in AtomSite vengono interrotti dopo il download di un sito pubblicato

> **Soluzione alternativa**  
> Nel file *Service. config* , nel file *Users. config* e in tutti i file *XML* , sostituire la stringa dell'URL (ad esempio, `http://myhost.com/atomsite`) con quella locale, ad esempio `http://localhost:1239`.

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: le applicazioni basate su MySQL come WordPress non riescono a pubblicare e segnalano un errore di database

> Per impostazione predefinita, WebMatrix installa MySQL con il set di caratteri UTF-8. Se si installa MySQL autonomamente e il set di caratteri non è UTF-8 (ad esempio, è Latin1), il processo di pubblicazione per i database potrebbe non riuscire.
> 
> **Soluzione alternativa**
> 
> 1. Modificare il set di caratteri per MySQL in UTF-8. Per informazioni dettagliate, vedere [set di caratteri del server e regole di confronto](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) nel sito web MySQL.
> 2. Reinstallare l'applicazione.
> 3. Ripubblicare l'applicazione.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "scaricare il sito pubblicato" non riesce per le applicazioni con installazione basata su browser

> Per alcune applicazioni, ad esempio Kentico CMS, è necessario avviarle nel browser per eseguire la configurazione successiva all'installazione, ad esempio la creazione di un database. Se si pubblica un'applicazione come questa senza completare la configurazione basata su browser, il tentativo di scaricare lo stesso sito da un server remoto avrà esito negativo.
> 
> **Soluzione alternativa**  
> Completare l'installazione basata su browser prima di pubblicare il sito.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "scaricare il sito pubblicato" ha esito negativo con un errore di database per DotNetNuke e Kooboo CMS

> Se si tenta di scaricare un'applicazione da un server e si dispone di credenziali di amministratore nella stringa di connessione del database nella finestra di dialogo **impostazioni di pubblicazione** , è possibile che nel log di pubblicazione venga visualizzato l'errore seguente:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Soluzione alternativa**  
> Se è possibile, ripubblicare il sito o pubblicarlo utilizzando credenziali non di amministratore per il database.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix 1,0, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tutti i diritti riservati. [Condizioni](https://msdn.microsoft.cos/cc300389.aspx)per l'utilizzo.
