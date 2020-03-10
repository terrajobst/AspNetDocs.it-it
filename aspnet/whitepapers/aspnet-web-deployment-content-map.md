---
uid: whitepapers/aspnet-web-deployment-content-map
title: Distribuzione Web ASP.NET-risorse consigliate | Microsoft Docs
author: rick-anderson
description: In questo argomento vengono forniti i collegamenti alle risorse della documentazione su come distribuire (pubblicare) applicazioni Web ASP.NET in IIS usando Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636990"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>Distribuzione Web ASP.NET - Risorse consigliate

> In questo argomento vengono forniti i collegamenti alle risorse della documentazione su come distribuire (pubblicare) applicazioni Web ASP.NET in IIS utilizzando Visual Studio 2010, Visual Web Developer 2010 e versioni successive.
> 
> Se si conosce un post di Blog straordinario, un thread [StackOverflow](http://stackoverflow.com) o qualsiasi altro collegamento utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) con il collegamento.
> 
> > [!NOTE] 
> > 
> > Molte di queste risorse descrivono le funzionalità di distribuzione disponibili solo se si installa una versione recente dell' [aggiornamento pubblicazione sul Web di Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Alcune funzionalità sono disponibili solo in Visual Studio 2012 o Visual Studio 2013.

Questo argomento è suddiviso nelle sezioni seguenti:

- [Informazioni sulle opzioni di distribuzione per i progetti Web](#understanding)
- [Ricerca di provider di hosting per un'applicazione ASP.NET](#findinghosting)
- [Distribuzione di un'applicazione Web da Visual Studio](#fromvs)
- [Distribuzione di un'applicazione Web tramite la creazione e l'installazione di un pacchetto di distribuzione Web](#package)
- [Distribuzione di un'applicazione Web tramite un processo di integrazione continua (CI)](#ci)
- [Uso delle trasformazioni di Web. config per modificare le impostazioni nel file Web. config o nel file app. config di destinazione durante la distribuzione](#transforms)
- [Utilizzo di parametri di Distribuzione Web per modificare le impostazioni nell'applicazione Web di destinazione durante la distribuzione](#webdeployparms)
- [Assicurarsi che un'applicazione non sia in linea durante la distribuzione](#appoffline)
- [Distribuzione di un database o di modifiche a un database come parte della distribuzione di applicazioni Web](#databasewithweb)
- [Distribuzione di un database separatamente dalla distribuzione di applicazioni Web](#databaseseparate)
- [Distribuzione di un'applicazione Web che usa i servizi delle applicazioni ASP.NET, ad esempio l'appartenenza e la profilatura](#aspnetmembership)
- [Precompilazione per la distribuzione](#precompiling)
- [Distribuzione di un'applicazione Web Intranet](#intranet)
- [Automazione delle attività di distribuzione comuni non automatizzate](#automating)
- [Configurazione di server Web in modo che gli sviluppatori possano distribuire le applicazioni Web usando Distribuzione Web](#configuringservers)
- [Configurazione dei server per un provider di hosting](#hostingprovider)
- [Risoluzione dei problemi di distribuzione](#troubleshooting)
- [Ottenere supporto per una domanda di distribuzione specifica](#gettinghelp)
- [Risorse aggiuntive](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Informazioni sulle opzioni di distribuzione per i progetti Web

- [Panoramica della distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Come distribuire un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Vengono illustrate le opzioni e i collegamenti alle risorse per la distribuzione di progetti Web in siti Web di Microsoft Azure, tra cui il [recapito continuo](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizzato dal [controllo del codice sorgente](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) e l'utilizzo di Visual Studio.
- [Miglioramenti alla pubblicazione sul Web di Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (video di Scott hanselr).
- [Post di panoramica per la distribuzione Web in Visual studio 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Blog di Luca Joshi). Un post di Blog precedente ma alcune delle risorse di Visual Studio 2010 a cui si collegano le informazioni che sono ancora rilevanti per Visual Studio 2012.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Ricerca di provider di hosting per un'applicazione ASP.NET

- [Hosting di ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Distribuzione di un'applicazione Web da Visual Studio

- [Come distribuire un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Vengono illustrate le opzioni e vengono forniti collegamenti alle risorse per la distribuzione di progetti Web in siti Web di Microsoft Azure. Include una sezione sulla distribuzione da Visual Studio.
- [Distribuzione di Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). la serie di esercitazioni in 12 parti illustra come distribuire applicazioni Web con database SQL Server. Per la distribuzione di database usa il provider dbDacFx e Migrazioni Code First di Entity Framework. Include anche informazioni sulle [trasformazioni di file Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), sulla [distribuzione di singoli file](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), sulla [distribuzione da riga di comando](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)e [su come personalizzare la pipeline di pubblicazione Web di Visual Studio modificando i file. pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Si applica a tutti i progetti Web ASP.NET, inclusi Web Form, MVC e API Web.
- [Procedura: distribuire un progetto Web tramite la pubblicazione con un clic in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informazioni di riferimento per la pubblicazione guidata sul Web di Visual Studio).
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Si tratta di una versione precedente della **distribuzione Web ASP.NET con Visual Studio** elencata all'inizio di questa sezione. Particolarmente utile per informazioni su come distribuire i database di SQL Server Compact e su come eseguire la migrazione da SQL Server Compact a un'edizione completa di SQL Server.
- [Applicazione .NET multilivello con tabelle, code e BLOB di archiviazione](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito di Microsoft Azure). la serie di esercitazioni in 5 parti illustra come creare un progetto MVC e distribuirlo in un servizio cloud di Windows Azure.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Distribuzione di un'applicazione Web tramite la creazione e l'installazione di un pacchetto di distribuzione Web

- [Procedura: creare un pacchetto di distribuzione Web in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Procedura: installare un pacchetto di distribuzione utilizzando il file Deploy. cmd creato da Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Uso di un pacchetto di distribuzione Web per la distribuzione in IIS nella casella dev e in un host di terze parti](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (Blog di Sayed Hashimi). Come utilizzare Gestione IIS per installare un pacchetto di distribuzione in IIS nel computer locale e in una società di hosting che supporta Gestione IIS per l'amministrazione remota.
- [Creazione di un pacchetto di distribuzione Web da Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (sito Web IIS.NET). Include istruzioni per la creazione e l'installazione di pacchetti da riga di comando.
- [Pacchetto dopo la pubblicazione in qualsiasi](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) posizione (Blog di Sayed Hashimi). Introduce un pacchetto NuGet che consente di automatizzare il processo di trasformazione del file Web. config per più ambienti di destinazione, in modo da poter distribuire un pacchetto a più server. Vedere anche il [video di PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) di Sayed Hashimi.

Vedere anche la sezione seguente.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Distribuzione di un'applicazione Web tramite un processo di integrazione continua (CI)

- [Integrazione continua e recapito continuo (compilazione di app Cloud reali con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capitolo e-book che introduce l'integrazione continua e il recapito continuo.
- [Come distribuire un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Vengono illustrate le opzioni e i collegamenti alle risorse per la distribuzione di progetti Web in siti Web di Microsoft Azure. Include una sezione sull'automazione della distribuzione dal controllo del codice sorgente.
- [Distribuzione di applicazioni Web in scenari aziendali](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). la serie di esercitazioni sulla parte 40 illustra come automatizzare la distribuzione in un processo CI usando Visual Studio 2010 e Team Foundation Server 2010.
- [All'interno del Microsoft Build Engine: usando MSBuild e Team Foundation Build, di Sayed Hashimi e William Bartholomew](http://msbuildbook.com). Si tratta di un libro, non di una risorsa Web, ma è una guida essenziale per apprendere come configurare MSBuild per gli scenari di integrazione continua.
- [Pacchetto di estensione MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Include le attività di distribuzione.
- [Guida alla personalizzazione di Team Foundation Build](https://aka.ms/vsarsolutions). Documentazione di ALM Rangers sulla configurazione di Team Foundation Server illustra la distribuzione Web e include esercitazioni e video.
- [Trasformazioni XML SlowCheetah da un server ci](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Blog di Sayed Hashimi). Viene illustrato come usare SlowCheetah, un componente aggiuntivo di Visual Studio per la trasformazione di app. config e di altri file XML.

Vedere anche [assicurarsi che un'applicazione non sia in linea durante la distribuzione](aspnet-web-deployment-content-map.md#appoffline) più avanti in questa pagina.

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Uso delle trasformazioni di Web. config per modificare le impostazioni nel file Web. config o nel file app. config di destinazione durante la distribuzione

- [Trasformazioni del file Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintassi di trasformazione di Web. config per la distribuzione del progetto Web con Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012,2-trasformazioni di Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (video di YouTube di Sayed Hashimi). Viene illustrato come impostare e visualizzare in anteprima le trasformazioni di Web. config.
- [Ricerca per categorie disabilitare la trasformazione Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quando è consigliabile utilizzare Distribuzione Web parametri anziché le trasformazioni di Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Xdt (trasformazione di documenti XML) rilasciata in CodePlex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (Blog su strumenti e sviluppo Web .NET). Annuncia la disponibilità del codice sorgente per il motore di trasformazione del file Web. config ed elenca alcuni strumenti che lo usano.
- [Siti Web di Microsoft Azure: funzionamento delle stringhe delle applicazioni e delle stringhe di connessione](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure Blog). Un'alternativa a Web. config viene trasformata se l'ambiente di destinazione è siti Web di Microsoft Azure e si desidera trasformare `appSettings` o `connectionStrings`.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Utilizzo di parametri di Distribuzione Web per modificare le impostazioni nell'applicazione Web di destinazione durante la distribuzione

- [Procedura: utilizzare parametri di distribuzione Web in un pacchetto di distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: come aggiornare le impostazioni dell'app durante la pubblicazione in base al profilo di pubblicazione](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (Blog di Sayed Hashimi). Viene illustrato come integrare i parametri di distribuzione Web nei profili di pubblicazione di Visual Studio.
- [Parametrizzazione distribuzione Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (sito Web IIS.NET).
- [Distribuzione Web parametrizzazione in azione](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Blog di Luca Joshi).
- [Distribuzione Web la parametrizzazione rispetto alla trasformazione Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Blog di Luca Joshi).
- [Siti Web di Microsoft Azure: funzionamento delle stringhe delle applicazioni e delle stringhe di connessione](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure Blog). Alternativa ai parametri di distribuzione Web se l'ambiente di destinazione è siti Web di Microsoft Azure e si desidera parametrizzare `appSettings` o `connectionStrings`.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Assicurarsi che un'applicazione non sia in linea durante la distribuzione

- [Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Vedere la sezione **portare offline l'applicazione durante la distribuzione.**
- [Portare offline un'applicazione prima della pubblicazione](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (sito di IIS.NET). Viene illustrata una funzionalità incorporata in Distribuzione Web 3,0 che consente di automatizzare la gestione di un'app\_file offline. htm. Questa funzionalità non funziona con l'app personalizzata\_file con estensione htm offline.
- [Come portare offline l'app Web durante la pubblicazione](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (Blog di Sayed Hashimi). Come automatizzare il processo di uso di un'app personalizzata\_file offline. htm.
- [Aggiornamenti di pubblicazione sul Web per app offline e useCheckSum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Blog sullo sviluppo Web Microsoft). Un'altra opzione per automatizzare l'uso dell'app\_file offline. htm.
- [Distribuzione Web 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (sito IIS.NET). Nuova funzionalità di Distribuzione Web 3,5 per app personalizzate\_file con estensione htm offline.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Distribuzione di un database o di modifiche a un database come parte della distribuzione di applicazioni Web

- [Configurazione della distribuzione del database in Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Panoramica delle opzioni per la distribuzione di un database con un progetto Web.
- [Distribuzione di Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). la serie di esercitazioni in 12 parti Mostra la distribuzione del database usando il provider dbDacFx e Migrazioni Code First di Entity Framework.
- [Procedura: distribuire un progetto Web tramite la pubblicazione con un clic in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione estesa che compila e distribuisce un'applicazione che utilizza un singolo database di SQL Server per l'appartenenza e i dati dell'applicazione.
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). la serie di esercitazioni in 12 parti illustra come distribuire database SQL Server Compact e come eseguire la migrazione da SQL Server Compact a un'edizione completa di SQL Server.

Vedere anche distribuzione di un'applicazione Web tramite la creazione e l'installazione di un pacchetto di distribuzione Web e la distribuzione di un'applicazione Web usando un processo di integrazione continua (CI) in precedenza in questa pagina.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Distribuzione di un database separatamente dalla distribuzione di applicazioni Web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Inclusione di dati in un progetto di database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (Blog SQL Server Data Tools Team). Come distribuire lo schema e i dati quando si distribuisce un database.
- [Come distribuire un database in Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (sito Microsoft Azure)
- [Migrazione di database al database SQL di Windows Azure (in precedenza SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrazione di un database a SQL Azure mediante SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (Blog del team SQL Server Data Tools).
- [Migrazione di applicazioni incentrate sui dati a Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrazione di database di SQL Server al database SQL di Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Distribuzione di un'applicazione Web che usa i servizi delle applicazioni ASP.NET, ad esempio l'appartenenza e la profilatura

- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione estesa che compila e distribuisce un'applicazione che utilizza un singolo database di SQL Server per l'appartenenza e i dati dell'applicazione.
- [ASP.NET Identity](https://asp.net/identity/). Risorse per ASP.NET Identity.
- [Distribuzione di Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). la serie di esercitazioni in 12 parti illustra come distribuire un database di appartenenza a ASP.NET.
- [Configurazione di un sito Web che usa servizi per le applicazioni](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Per i progetti di siti Web, ma è pertinente anche per i progetti di applicazioni Web.
- [Utenti e ruoli nel sito Web di produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Per i progetti di siti Web, ma è pertinente anche per i progetti di applicazioni Web.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Precompilazione per la distribuzione

- [Panoramica della precompilazione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Scheda pubblicazione/pacchetto Web, proprietà progetto](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- Finestra di [dialogo Impostazioni avanzate di precompilazione](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Distribuzione di un'applicazione Web Intranet

- [Usare l'opzione di autenticazione organizzativa locale (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog di Vittorio Bertocci).
- [Creazione di un sito Intranet mediante ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). La procedura dettagliata precedente scritta per Visual Studio 2010 non riflette le principali modifiche apportate ai modelli di progetto Intranet introdotti in Visual Studio 2013.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automazione delle attività di distribuzione comuni non automatizzate

- [Distribuzione Web ASP.NET con Visual Studio: distribuzione di file aggiuntivi](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Impostazione delle autorizzazioni per le cartelle sul Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (Blog di Sayed Hashimi).
- [Come estendere il file di destinazioni in modo da includere le impostazioni del registro di sistema per un pacchetto di progetto Web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (Blog sugli strumenti di sviluppo Web).
- [Estensione della trasformazione XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (Blog di Sayed Hashimi). Viene illustrato come creare trasformazioni XDT personalizzate.
- Il [provider personalizzato MSDeploy (Web Deployment Tool) accetta 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Blog di Sayed Hashimi). Viene illustrato come creare un Distribuzione Web provider personalizzato.
- [Come creare pacchetti e distribuire componenti com](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Blog sugli strumenti di sviluppo Web).
- [Come creare un pacchetto degli assembly .NET](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (Blog sugli strumenti di sviluppo Web). Come distribuire gli assembly nella GAC.
- Genera [script per tutto: Inizializza la macchina virtuale Windows Azure per il server Web con IIS, distribuzione Web e altro](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (Blog di Tugberk Ugurlu).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configurazione di server Web in modo che gli sviluppatori possano distribuire le applicazioni Web usando Distribuzione Web

- [Installazione e configurazione di distribuzione Web per distribuzioni di amministratori e non amministratori](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (sito di IIS.NET).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Configurazione dei server per un provider di hosting

- [Guida alla distribuzione dell'hosting di Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (area download Microsoft).
- [Genera un file XML del profilo](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (sito IIS.NET).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Risoluzione dei problemi di distribuzione

- [Risoluzione dei problemi relativi ai siti Web di Microsoft Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (sito Microsoft Azure).
- [Distribuzione Web ASP.NET con Visual Studio: risoluzione dei problemi](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Risoluzione dei problemi comuni con distribuzione Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Codici di errore distribuzione Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (sito IIS.NET).
- [Domande frequenti sulla distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Differenze principali tra IIS e il server di sviluppo ASP.NET](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Differenze di configurazione comuni tra sviluppo e produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosting di applicazioni ASP.NET con attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 persone dal sito Rolla).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Ottenere supporto per una domanda di distribuzione specifica

- [Forum sulla configurazione e sulla distribuzione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

In questa sezione vengono forniti collegamenti a risorse aggiuntive utili per ottenere ulteriori informazioni sull'utilizzo degli strumenti di distribuzione di Visual Studio e IIS.

I blog seguenti contengono di frequente informazioni sulla distribuzione Web di Visual Studio:

- [Strumenti di sviluppo Web nel Blog di Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog di Sayed Hashimi](http://www.sedodream.com/).

Le risorse seguenti forniscono la documentazione relativa a Distribuzione Web, il framework IIS usato da Visual Studio per eseguire le attività di distribuzione del progetto di applicazione Web. È possibile porre domande su Distribuzione Web nel [forum dello strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=149411) nel sito Web IIS.NET.

- [Introduzione a distribuzione Web](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installazione e configurazione di distribuzione Web](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Script di PowerShell per automatizzare l'installazione di distribuzione Web](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=151481). Nodo Sommario di primo livello per la documentazione Distribuzione Web sul sito TechNet. Include informazioni di riferimento utili ma la maggior parte delle pagine TechNet non è stata aggiornata per anni.
- [Spazio dei nomi Microsoft. Web. Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). La documentazione dell'API non è stata aggiornata dalla versione 1,0.
- [Blog del team di distribuzione Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Scheda pubblica nel sito web IIS.NET](https://www.iis.net/learn/publish).
