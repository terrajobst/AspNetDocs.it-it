---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio: Introduzione-1 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525634"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio: Introduzione-1 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web.
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Queste esercitazioni consentono di eseguire prima la distribuzione in IIS nel computer di sviluppo locale per i test e quindi a un provider di hosting di terze parti. L'applicazione che verrà distribuita utilizzerà un database dell'applicazione e un database di appartenenza a ASP.NET. Si inizia con SQL Server Compact e si distribuisce in SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche al database e come eseguire la migrazione a SQL Server.
> 
> Le esercitazioni presuppongono che si conoscano le modalità di utilizzo di ASP.NET in Visual Studio. In caso contrario, un punto di partenza valido è un'esercitazione [Web Forms di base di ASP.NET](../tailspin-spyworks/tailspin-spyworks-part-1.md) o un'esercitazione di base su [ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel forum sulla distribuzione di [ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).

## <a name="overview"></a>Panoramica

Queste esercitazioni consentono di eseguire prima la distribuzione in IIS nel computer di sviluppo locale per i test e quindi a un provider di hosting di terze parti. L'applicazione che verrà distribuita utilizzerà un database dell'applicazione e un database di appartenenza a ASP.NET. Si inizia con SQL Server Compact e si distribuisce in SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche al database e come eseguire la migrazione a SQL Server.

Il numero di esercitazioni – 11 in tutti più una pagina di risoluzione dei problemi, potrebbe rendere il processo di distribuzione alquanto scoraggiante. Infatti, le procedure di base per la distribuzione di un sito costituiscono una parte relativamente piccola del set di esercitazioni. Tuttavia, in situazioni reali, è spesso necessario ottenere informazioni su alcuni aspetti molto piccoli ma importanti della distribuzione, ad esempio l'impostazione delle autorizzazioni per le cartelle nel server di destinazione. Nelle esercitazioni sono state incluse molte di queste tecniche aggiuntive, con la speranza che le esercitazioni non lascino informazioni che potrebbero impedire la corretta distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per essere eseguite in sequenza e ogni parte si basa sulla parte precedente. Tuttavia, è possibile ignorare le parti che non sono rilevanti per la situazione. Per ignorare le parti potrebbe essere necessario modificare le procedure nelle esercitazioni successive.

## <a name="intended-audience"></a>Destinatari del documento

Le esercitazioni sono destinate agli sviluppatori ASP.NET che lavorano in organizzazioni di piccole dimensioni o in altri ambienti in cui:

- Non viene utilizzato un processo di integrazione continua (compilazioni e distribuzione automatiche).
- L'ambiente di produzione è un provider di hosting di terze parti.
- Una persona riempie in genere più ruoli (la stessa persona sviluppa, verifica e distribuisce).

Negli ambienti aziendali, è più tipico implementare processi di integrazione continua e l'ambiente di produzione è generalmente ospitato dai server aziendali. Anche le persone diverse eseguono in genere ruoli diversi. Per informazioni sulla distribuzione aziendale, vedere [distribuzione di applicazioni Web in scenari aziendali](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Le organizzazioni di qualsiasi dimensione possono anche distribuire applicazioni Web in Azure e la maggior parte delle procedure illustrate in queste esercitazioni si applicano anche alle app Web di app Azure Services. Per un'introduzione ad Azure, vedere [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Il provider di hosting illustrato nelle esercitazioni

Le esercitazioni illustrano il processo di configurazione di un account con una società di hosting e la distribuzione dell'applicazione nel provider di hosting. È stata scelta una società di hosting specifica in modo che le esercitazioni possano illustrare l'esperienza completa di distribuzione in un sito Web attivo. Ogni società di hosting fornisce funzionalità diverse e l'esperienza di distribuzione nei server varia leggermente; Tuttavia, il processo descritto in questa esercitazione è tipico per il processo complessivo.

Il provider di hosting usato per questa esercitazione, Cytanium.com, è uno dei molti disponibili e il suo utilizzo in questa esercitazione non costituisce un'approvazione o una raccomandazione.

## <a name="deploying-web-site-projects"></a>Distribuzione di progetti di siti Web

Contoso University è un progetto di applicazione Web di Visual Studio. La maggior parte degli strumenti e dei metodi di distribuzione illustrati in questa esercitazione non si applicano ai [progetti di siti Web](https://msdn.microsoft.com/library/dd547590.aspx). Per informazioni sulla distribuzione di progetti di siti Web, vedere la pagina relativa alla [mappa dei contenuti di ASP.NET Deployment](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti MVC ASP.NET

Per questa esercitazione si distribuisce un progetto Web Form ASP.NET, ma tutto ciò che si apprende è applicabile anche a ASP.NET MVC. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione Web. L'unica differenza è che se si esegue la distribuzione in un provider di hosting che non supporta ASP.NET MVC o la versione di destinazione, è necessario assicurarsi di aver installato il pacchetto NuGet ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) appropriato nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio C# utilizza, ma le esercitazioni non richiedono C#alcuna conoscenza di e le tecniche di distribuzione illustrate nelle esercitazioni non sono specifiche della lingua.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante questa esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non forniscono sempre una soluzione. Per semplificare gli scenari comuni di problemi, è disponibile una pagina di riferimento per la [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) . Se viene ricevuto un messaggio di errore o un elemento non funziona durante le esercitazioni, assicurarsi di controllare la pagina relativa alla risoluzione dei problemi.

## <a name="comments-welcome"></a>Commenti Benvenuti

I commenti sulle esercitazioni sono benvenuti e, quando l'esercitazione viene aggiornata, verranno presi in considerazione le correzioni o i suggerimenti per i miglioramenti offerti nei commenti dell'esercitazione.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare di disporre di Windows 7 o versione successiva e di uno dei seguenti prodotti installati nel computer:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Se si dispone di Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, installare anche i prodotti seguenti:

- [Azure SDK per .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (include l'aggiornamento di pubblicazione sul Web)
- [Strumenti di Microsoft Visual Studio 2010 SP1 per SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Per completare l'esercitazione è necessario un altro software, ma non è necessario che sia ancora stato caricato. Questa esercitazione illustra i passaggi necessari per l'installazione.

## <a name="downloading-the-sample-application"></a>Download dell'applicazione di esempio

L'applicazione da distribuire è denominata Contoso University ed è già stata creata automaticamente. Si tratta di una versione semplificata di un sito Web dell'Università, basata liberamente sull'applicazione Contoso University descritta nella [Entity Framework esercitazioni sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare l' [applicazione Web di Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Il file *zip* contiene più versioni del progetto e un file PDF che contiene tutte e 12 le esercitazioni. Per eseguire i passaggi dell'esercitazione, iniziare con ContosoUniversity-Begin. Per visualizzare il progetto simile alla fine delle esercitazioni, aprire ContosoUniversity-end. Per visualizzare l'aspetto del progetto prima della migrazione alla SQL Server completa nell'esercitazione 10, aprire ContosoUniversity-AfterTutorial09.

Per prepararsi a eseguire i passaggi dell'esercitazione, salvare ContosoUniversity-Begin in qualsiasi cartella usata per l'uso dei progetti di Visual Studio. Per impostazione predefinita, si tratta della cartella seguente:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

Per le schermate di questa esercitazione, la cartella del progetto si trova nella directory radice dell'unità `C`:.

Avviare Visual Studio, aprire il progetto e premere CTRL + F5 per eseguirlo.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Le pagine del sito Web sono accessibili dalla barra dei menu e consentono di eseguire le funzioni seguenti:

- Visualizza le statistiche degli studenti (pagina about).
- Visualizza, modifica, Elimina e Aggiungi studenti.
- Visualizza e modifica i corsi.
- Visualizza e modifica gli istruttori.
- Visualizzare e modificare i reparti.

Di seguito sono riportate alcune schermate di alcune pagine rappresentative.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisione delle funzionalità dell'applicazione che influiscono sulla distribuzione

Le funzionalità seguenti dell'applicazione influiscono sulla modalità di distribuzione o sulle operazioni da eseguire per la distribuzione. Ognuna di queste informazioni è illustrata più dettagliatamente nelle esercitazioni seguenti della serie.

- Contoso University USA un database di SQL Server Compact per archiviare i dati dell'applicazione, ad esempio i nomi degli studenti e degli istruttori. Il database contiene una combinazione di dati di test e dati di produzione e quando si esegue la distribuzione nell'ambiente di produzione è necessario escludere i dati di test. Più avanti nella serie di esercitazioni si eseguirà la migrazione da SQL Server Compact a SQL Server.
- L'applicazione usa il sistema di appartenenze ASP.NET, che archivia le informazioni sull'account utente in un database SQL Server Compact. L'applicazione definisce un utente amministratore che ha accesso ad alcune informazioni limitate. È necessario distribuire il database delle appartenenze senza account di test, ma con un account amministratore.
- Poiché il database dell'applicazione e il database di appartenenza utilizzano SQL Server Compact come motore di database, è necessario distribuire il motore di database al provider di hosting, nonché i database stessi.
- L'applicazione usa i provider di appartenenze ASP.NET universali in modo che il sistema di appartenenze possa archiviare i dati in un database SQL Server Compact. L'assembly che contiene i provider di appartenenze universali deve essere distribuito con l'applicazione.
- L'applicazione usa la Entity Framework 5,0 per accedere ai dati nel database dell'applicazione. L'assembly che contiene Entity Framework 5,0 deve essere distribuito con l'applicazione.
- L'applicazione utilizza un'utilità di registrazione e creazione di report degli errori di terze parti. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione degli errori scrive le informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account con cui viene eseguito ASP.NET nel sito distribuito disponga delle autorizzazioni di scrittura per questa cartella ed è necessario escludere questa cartella dalla distribuzione. In caso contrario, i dati del log degli errori dell'ambiente di test potrebbero essere distribuiti in produzione e/o file di log degli errori di produzione potrebbero essere eliminati.
- Nell'applicazione sono incluse alcune impostazioni che devono essere modificate nel file *Web. config* distribuito, a seconda dell'ambiente di destinazione (test o produzione) e di altre impostazioni che devono essere modificate a seconda della configurazione della compilazione (debug o rilascio).
- La soluzione di Visual Studio include un progetto libreria di classi. È necessario distribuire solo l'assembly generato dal progetto, non il progetto stesso.

In questa prima esercitazione della serie è stato scaricato il progetto di Visual Studio di esempio e sono state rivedute le funzionalità del sito che influiscono sulla modalità di distribuzione dell'applicazione. Nelle esercitazioni seguenti si prepara la distribuzione impostando alcuni di questi elementi da gestire automaticamente. Altri utenti che si occupano manualmente.

> [!div class="step-by-step"]
> [avanti](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
