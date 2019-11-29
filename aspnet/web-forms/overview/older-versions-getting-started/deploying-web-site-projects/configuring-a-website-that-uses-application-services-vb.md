---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configurazione di un sito Web che usa Servizi per le applicazioni (VB) | Microsoft Docs
author: rick-anderson
description: Con la versione 2,0 di ASP.NET è stata introdotta una serie di servizi applicativi, che fanno parte del .NET Framework e vengono usati come una suite di servizi di blocco costitutivi.
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588732"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Configurazione di un sito Web che usa i servizi per le applicazioni (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> Con la versione 2,0 di ASP.NET è stata introdotta una serie di servizi applicativi, che fanno parte del .NET Framework e vengono usati come una suite di servizi di blocco predefiniti che è possibile usare per aggiungere funzionalità avanzate all'applicazione Web. Questa esercitazione illustra come configurare un sito Web nell'ambiente di produzione per usare i servizi dell'applicazione e risolve i problemi comuni relativi alla gestione di account utente e ruoli nell'ambiente di produzione.

## <a name="introduction"></a>Introduzione

Con la versione 2,0 di ASP.NET è stata introdotta una serie di *servizi applicativi*, che fanno parte del .NET Framework e vengono usati come una suite di servizi di blocco predefiniti che è possibile usare per aggiungere funzionalità avanzate all'applicazione Web. I servizi dell'applicazione includono:

- **Appartenenza** : un'API per la creazione e la gestione degli account utente.
- **Roles** : un'API per la categorizzazione degli utenti in gruppi.
- **Profile** : un'API per l'archiviazione di contenuto personalizzato specifico dell'utente.
- **Mappa del sito** : un'API per la definizione di una struttura logica del sito sotto forma di gerarchia, che può essere visualizzata tramite controlli di navigazione, ad esempio menu e percorsi di navigazione.
- **Personalizzazione** : un'API per la gestione delle preferenze di personalizzazione, usata più spesso con [*WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitoraggio dello stato** : un'API per il monitoraggio delle prestazioni, della sicurezza, degli errori e di altre metriche di integrità del sistema per un'applicazione Web in esecuzione.

Le API dei servizi dell'applicazione non sono associate a un'implementazione specifica. Al contrario, si indica ai servizi dell'applicazione di utilizzare un determinato *provider*e tale provider implementa il servizio utilizzando una tecnologia particolare. I provider usati più di frequente per le applicazioni Web basate su Internet ospitate in una società di hosting Web sono i provider che usano un'implementazione di database SQL Server. Ad esempio, il `SqlMembershipProvider` è un provider per l'API di appartenenza che archivia le informazioni sull'account utente in un database Microsoft SQL Server.

Quando si distribuisce l'applicazione, l'utilizzo dei servizi dell'applicazione e dei provider di SQL Server comporta problemi. Per i principianti, gli oggetti di database dei servizi dell'applicazione devono essere creati correttamente nei database di sviluppo e di produzione e inizializzati in modo appropriato. Sono inoltre importanti le impostazioni di configurazione che devono essere eseguite.

> [!NOTE]
> Le API dei servizi delle applicazioni sono state progettate usando il [*modello di provider*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), uno schema progettuale che consente di fornire i dettagli dell'implementazione di un'API in fase di esecuzione. Il .NET Framework viene fornito con un numero di provider di servizi dell'applicazione che è possibile usare, ad esempio i `SqlMembershipProvider` e `SqlRoleProvider`, ovvero i provider per le API di appartenenza e ruoli che usano un'implementazione di database SQL Server. È anche possibile creare e collegare un provider personalizzato. Infatti, l'applicazione Web di revisione del libro contiene già un provider personalizzato per l'API della mappa del sito (`ReviewSiteMapProvider`), che costruisce la mappa del sito dai dati nelle tabelle `Genres` e `Books` nel database.

Questa esercitazione inizia con un'analisi della modalità di estensione dell'applicazione Web revisioni del libro per l'uso delle API di appartenenza e dei ruoli. Viene quindi illustrata la distribuzione di un'applicazione Web che utilizza i servizi delle applicazioni con un'implementazione di database SQL Server e si conclude risolvendo i problemi comuni relativi alla gestione degli account utente e dei ruoli nell'ambiente di produzione.

## <a name="updates-to-the-book-reviews-application"></a>Aggiornamenti all'applicazione Book revisioni

Negli ultimi due esercitazioni l'applicazione Web revisioni del libro è stata aggiornata da un sito Web statico a un'applicazione Web dinamica basata sui dati completa con un set di pagine di amministrazione per la gestione di generi e revisioni. Tuttavia, questa sezione di amministrazione non è attualmente protetta, ovvero qualsiasi utente che conosce (o indovina) l'URL della pagina di amministrazione può entrare in valzer e creare, modificare o eliminare recensioni nel sito. Un modo comune per proteggere determinate parti di un sito Web consiste nell'implementare gli account utente e quindi utilizzare le regole di autorizzazione URL per limitare l'accesso a determinati utenti o ruoli. L'applicazione Web Book revisioni disponibile per il download in questa esercitazione supporta account utente e ruoli. Dispone di un singolo ruolo definito amministratore e solo gli utenti con questo ruolo possono accedere alle pagine di amministrazione.

> [!NOTE]
> Ho creato tre account utente in Book revisioni applicazione Web: Scott, Jisun e Alice. Tutti e tre gli utenti hanno la stessa password: **password.** Scott e Jisun sono nel ruolo di amministratore. Alice non lo è. Le pagine non di amministrazione del sito sono ancora accessibili agli utenti anonimi. Ovvero, non è necessario eseguire l'accesso per visitare il sito, a meno che non si desideri amministrarlo, nel qual caso è necessario accedere come utente con il ruolo di amministratore.

La pagina master dell'applicazione revisione libri è stata aggiornata in modo da includere un'interfaccia utente diversa per utenti autenticati e anonimi. Se un utente anonimo visita il sito, viene visualizzato un collegamento di accesso nell'angolo superiore destro. Un utente autenticato Visualizza il messaggio "Welcome back, *username*!" e un collegamento per la disconnessione. C'è anche una pagina di accesso (`~/Login.aspx`), che contiene un controllo Web di accesso che fornisce l'interfaccia utente e la logica per l'autenticazione di un visitatore. Solo gli amministratori possono creare nuovi account. Sono disponibili pagine per la creazione e la gestione degli account utente nella cartella `~/Admin`.

### <a name="configuring-the-membership-and-roles-apis"></a>Configurazione delle API di appartenenza e dei ruoli

L'applicazione Web Book revisioni usa le API di appartenenza e dei ruoli per supportare gli account utente e per raggruppare tali utenti in ruoli (ovvero il ruolo di amministratore). Vengono utilizzate le classi del provider `SqlMembershipProvider` e `SqlRoleProvider` perché si desidera archiviare le informazioni sull'account e sui ruoli in un database SQL Server.

> [!NOTE]
> Questa esercitazione non è destinata a un esame dettagliato della configurazione di un'applicazione Web per supportare le API di appartenenza e dei ruoli. Per informazioni dettagliate su queste API e sui passaggi da eseguire per configurare un sito Web per l'uso, vedere le [*esercitazioni sulla sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Per usare i servizi dell'applicazione con un database di SQL Server, è innanzitutto necessario aggiungere al database gli oggetti di database usati da tali provider per il database in cui si desidera archiviare le informazioni relative all'account utente e al ruolo. Questi oggetti di database necessari includono un'ampia gamma di tabelle, viste e stored procedure. Se non specificato diversamente, le classi del provider `SqlMembershipProvider` e `SqlRoleProvider` utilizzano un database SQL Server Express Edition denominato `ASPNETDB` che si trova nella cartella `App_Data` dell'applicazione; Se tale database non esiste, viene creato automaticamente con gli oggetti di database necessari da tali provider in fase di esecuzione.

È possibile, e in genere ideale, creare gli oggetti di database dei servizi dell'applicazione nello stesso database in cui sono archiviati i dati specifici dell'applicazione del sito Web. Il .NET Framework viene fornito con uno strumento denominato `aspnet_regsql.exe` che consente di installare gli oggetti di database in un database specificato. Ho utilizzato questo strumento per aggiungere questi oggetti al database `Reviews.mdf` nella cartella `App_Data` (database di sviluppo). Si vedrà come usare questo strumento più avanti in questa esercitazione quando si aggiungono questi oggetti al database di produzione.

Se si aggiungono gli oggetti di database dei servizi dell'applicazione a un database diverso da `ASPNETDB` sarà necessario personalizzare le configurazioni delle classi provider `SqlMembershipProvider` e `SqlRoleProvider` in modo che usino il database appropriato. Per personalizzare il provider di appartenenze, aggiungere un [ *&lt;elemento&gt; appartenenza*](https://msdn.microsoft.com/library/1b9hw62f.aspx) all'interno della sezione `<system.web>` in `Web.config`; utilizzare l' [*elemento&lt;roleManager&gt;* ](https://msdn.microsoft.com/library/ms164660.aspx) per configurare il provider di ruoli. Il frammento di codice seguente è tratto dal libro revisioni dell'applicazione `Web.config` e Mostra le impostazioni di configurazione per le API appartenenza e ruoli. Si noti che entrambi registrano un nuovo provider-`ReviewMembership` e `ReviewRole` che usano rispettivamente i provider `SqlMembershipProvider` e `SqlRoleProvider`.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Per supportare l'autenticazione basata su form è stato inoltre configurato l'elemento `<authentication>` di `Web.config` file.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitazione dell'accesso alle pagine di amministrazione

ASP.NET consente di concedere o negare l'accesso a un file o a una cartella specifica in base all'utente o al ruolo tramite la relativa funzionalità di *autorizzazione URL* . (Si è brevemente discusso l'autorizzazione dell'URL nelle *differenze principali tra IIS e l'esercitazione server di sviluppo ASP.NET* e viene illustrato come IIS e il server di sviluppo ASP.NET applicano le regole di autorizzazione URL in modo diverso per il contenuto statico e dinamico.) Poiché si vuole impedire l'accesso alla cartella `~/Admin` ad eccezione degli utenti nel ruolo di amministratore, è necessario aggiungere regole di autorizzazione URL a questa cartella. In particolare, le regole di autorizzazione dell'URL devono consentire agli utenti con il ruolo di amministratore di negare l'accesso a tutti gli altri utenti. Questa operazione viene eseguita aggiungendo un file di `Web.config` alla cartella `~/Admin` con il contenuto seguente:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Per altre informazioni sulla funzionalità di autorizzazione URL di ASP.NET e su come usarla per definire le regole di autorizzazione per gli utenti e per i ruoli, assicurarsi di leggere le esercitazioni sull'autorizzazione [*basata sull'utente*](../../older-versions-security/membership/user-based-authorization-vb.md) e [*sulle autorizzazioni basate sui*](../../older-versions-security/roles/role-based-authorization-vb.md) ruoli dalle [*esercitazioni sulla sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Distribuzione di un'applicazione Web che utilizza Servizi per le applicazioni

Quando si distribuisce un sito Web che usa i servizi delle applicazioni e un provider che archivia le informazioni dei servizi dell'applicazione in un database, è fondamentale che gli oggetti di database necessari per i servizi dell'applicazione vengano creati nel database di produzione. Inizialmente il database di produzione non contiene questi oggetti, pertanto quando l'applicazione viene distribuita per la prima volta (o quando viene distribuita per la prima volta dopo l'aggiunta dei servizi dell'applicazione), è necessario eseguire passaggi aggiuntivi per ottenere questi oggetti di database necessari nel database di produzione.

È possibile che si verifichi un'altra sfida quando si distribuisce un sito Web che usa i servizi delle applicazioni se si intende replicare gli account utente creati nell'ambiente di sviluppo nell'ambiente di produzione. A seconda della configurazione di appartenenza e ruoli, è possibile che anche se si copiano correttamente gli account utente creati nell'ambiente di sviluppo nel database di produzione, questi utenti non possono accedere all'applicazione Web in produzione. Si esaminerà la causa del problema e verrà illustrato come impedire che si verifichi.

ASP.NET viene fornito con un simpatico [*strumento di amministrazione di siti Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) che può essere avviato da Visual Studio e consente di gestire l'account utente, i ruoli e le regole di autorizzazione tramite un'interfaccia basata sul Web. Sfortunatamente, il WSAT funziona solo per i siti Web locali, ovvero non può essere usato per gestire in remoto gli account utente, i ruoli e le regole di autorizzazione per l'applicazione Web nell'ambiente di produzione. Si esamineranno diversi modi per implementare il comportamento di tipo WSAT dal sito Web di produzione.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Aggiunta di oggetti di database tramite ASPNET\_RegSql. exe

Nell'esercitazione sulla *distribuzione di un database* è stato illustrato come copiare le tabelle e i dati dal database di sviluppo al database di produzione. queste tecniche possono certamente essere utilizzate per copiare gli oggetti di database dei servizi dell'applicazione nel database di produzione. Un'altra opzione è lo strumento `aspnet_regsql.exe`, che aggiunge o rimuove gli oggetti di database dei servizi dell'applicazione da un database.

> [!NOTE]
> Lo strumento `aspnet_regsql.exe` crea gli oggetti di database in un database specificato. Non esegue la migrazione dei dati in tali oggetti di database dal database di sviluppo al database di produzione. Se si intende copiare l'account utente e le informazioni sui ruoli nel database di sviluppo nel database di produzione, usare le tecniche descritte nell'esercitazione *distribuzione di un database* .

Si osservi come aggiungere gli oggetti di database al database di produzione usando lo strumento `aspnet_regsql.exe`. Per iniziare, aprire Esplora risorse e passare alla directory .NET Framework versione 2,0 nel computer in uso,% WINDIR% \ Microsoft. NET\Framework\v2.0.50727. È necessario trovare lo strumento `aspnet_regsql.exe`. Questo strumento può essere usato dalla riga di comando, ma include anche un'interfaccia utente grafica. fare doppio clic sul file `aspnet_regsql.exe` per avviare il componente grafico.

Lo strumento inizia visualizzando una schermata iniziale che ne spiega lo scopo. Fare clic su Avanti per passare alla schermata "selezionare un'opzione di installazione", illustrata nella figura 1. Da qui è possibile scegliere di aggiungere gli oggetti di database dei servizi dell'applicazione o di rimuoverli da un database. Poiché si desidera aggiungere questi oggetti al database di produzione, selezionare l'opzione "configura SQL Server per servizi applicazioni" e fare clic su Avanti.

[![scegliere di configurare SQL Server per Servizi per le applicazioni](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figura 1**: scegliere di configurare SQL Server per servizi per le applicazioni ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

Nella schermata "selezionare il server e il database" vengono richieste informazioni per la connessione al database. Immettere il server di database, le credenziali di sicurezza e il nome del database fornito dall'azienda di hosting Web e fare clic su Avanti.

> [!NOTE]
> Dopo aver immesso il server di database e le credenziali, è possibile che venga ricevuto un errore durante l'espansione dell'elenco a discesa database. Lo strumento `aspnet_regsql.exe` esegue una query sulla tabella di sistema `sysdatabases` per recuperare un elenco di database nel server, ma alcune società di hosting Web bloccano i server di database in modo che queste informazioni non siano disponibili pubblicamente. Se viene ricevuto questo errore, è possibile digitare il nome del database direttamente nell'elenco a discesa.

[![fornire lo strumento con le informazioni di connessione del database](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figura 2**: fornire lo strumento con le informazioni di connessione al database ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

Nella schermata successiva vengono riepilogate le azioni che stanno per essere eseguite, ovvero gli oggetti di database dei servizi dell'applicazione verranno aggiunti al database specificato. Per completare l'azione, fare clic su Avanti. Dopo alcuni istanti, viene visualizzata la schermata finale, notando che gli oggetti di database sono stati aggiunti (vedere la figura 3).

[![esito positivo. Gli oggetti di database Servizi per le applicazioni sono stati aggiunti al database di produzione](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figura 3**: operazione riuscita. Gli oggetti di database Servizi per le applicazioni sono stati aggiunti al database di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))

Per verificare che gli oggetti di database dei servizi dell'applicazione siano stati aggiunti correttamente al database di produzione, aprire SQL Server Management Studio e connettersi al database di produzione. Come illustrato nella figura 4, verranno visualizzate le tabelle del database dei servizi dell'applicazione nel database, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`e così via.

[![verificare che gli oggetti di database siano stati aggiunti al database di produzione](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Figura 4**: verificare che gli oggetti di database siano stati aggiunti al database di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

È necessario usare lo strumento `aspnet_regsql.exe` solo quando si distribuisce l'applicazione Web per la prima volta o per la prima volta dopo aver iniziato a usare i servizi dell'applicazione. Quando questi oggetti di database si trovano nel database di produzione, non è necessario aggiungerli di nuovo o modificarli.

### <a name="copying-user-accounts-from-development-to-production"></a>Copia degli account utente dallo sviluppo alla produzione

Quando si utilizzano le classi del provider `SqlMembershipProvider` e `SqlRoleProvider` per archiviare le informazioni dei servizi dell'applicazione in un database SQL Server, le informazioni sull'account utente e sul ruolo vengono archiviate in un'ampia gamma di tabelle di database, tra cui `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`e `aspnet_UsersInRoles`. Se durante lo sviluppo si creano account utente nell'ambiente di sviluppo, è possibile replicare tali account utente in produzione copiando i record corrispondenti dalle tabelle di database applicabili. Se è stata utilizzata la pubblicazione guidata database per distribuire gli oggetti di database dei servizi applicazioni, è possibile che sia stata anche scelta la copia dei record, in modo che gli account utente creati in fase di sviluppo siano anche in produzione. Tuttavia, a seconda delle impostazioni di configurazione, è possibile che gli utenti i cui account sono stati creati in fase di sviluppo e copiati in produzione non siano in grado di accedere dal sito Web di produzione. Che cosa offre?

Le classi del provider `SqlMembershipProvider` e `SqlRoleProvider` sono state progettate in modo da consentire a un singolo database di fungere da archivio utente per più applicazioni, in cui ogni applicazione può, in teoria, avere utenti con nomi utente e ruoli sovrapposti con lo stesso nome. Per consentire questa flessibilità, il database gestisce un elenco di applicazioni nella tabella `aspnet_Applications` e ogni utente è associato a una di queste applicazioni. In particolare, la tabella `aspnet_Users` dispone di una colonna `ApplicationId` che associa ogni utente a un record nella tabella `aspnet_Applications`.

Oltre alla colonna `ApplicationId`, la tabella `aspnet_Applications` include anche una colonna `ApplicationName`, che fornisce un nome più descrittivo per l'applicazione. Quando un sito Web tenta di usare un account utente, ad esempio la convalida delle credenziali di un utente dalla pagina di accesso, deve indicare alla classe `SqlMembershipProvider` quale applicazione usare. Questa operazione viene in genere eseguita specificando il nome dell'applicazione e questo valore deriva dalla configurazione del provider in `Web.config`, in particolare tramite l'attributo `applicationName`.

Ma cosa accade se l'attributo `applicationName` non è specificato in `Web.config`? In tal caso, il sistema di appartenenze usa il percorso radice dell'applicazione come valore `applicationName`. Se l'attributo `applicationName` non viene impostato in modo esplicito in `Web.config`, è possibile che l'ambiente di sviluppo e l'ambiente di produzione usino una radice dell'applicazione diversa e pertanto verranno associati a nomi di applicazioni diversi nei servizi dell'applicazione. Se si verifica una mancata corrispondenza, gli utenti creati nell'ambiente di sviluppo avranno un valore `ApplicationId` che non corrisponde al valore `ApplicationId` per l'ambiente di produzione. Il risultato finale è che tali utenti non saranno in grado di eseguire l'accesso.

> [!NOTE]
> Se ci si trova in questa situazione: con gli account utente copiati nell'ambiente di produzione con un valore di `ApplicationId` non corrispondente, è possibile scrivere una query per aggiornare questi valori di `ApplicationId` non corretti al `ApplicationId` usato nell'ambiente di produzione. Al termine dell'aggiornamento, gli utenti i cui account sono stati creati nell'ambiente di sviluppo saranno ora in grado di accedere all'applicazione Web nell'ambiente di produzione.

L'aspetto positivo è che è possibile eseguire un semplice passaggio per assicurarsi che i due ambienti usino lo stesso `ApplicationId`, impostando in modo esplicito l'attributo `applicationName` in `Web.config` per tutti i provider di servizi dell'applicazione. Si imposta in modo esplicito l'attributo `applicationName` su "BookReviews" negli elementi `<membership>` e `<roleManager>` come il frammento di `Web.config` visualizzato.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Per altre informazioni sull'impostazione dell'attributo `applicationName` e sulla relativa logica, vedere il post di Blog di [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) , [*impostare sempre la proprietà ApplicationName quando si configura l'appartenenza a ASP.NET e altri provider*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gestione degli account utente nell'ambiente di produzione

Lo strumento Amministrazione sito Web di ASP.NET (WSAT) semplifica la creazione e la gestione degli account utente, la definizione e l'applicazione dei ruoli e l'ortografia delle regole di autorizzazione basate sugli utenti e sui ruoli. Per avviare il WSAT da Visual Studio, passare al Esplora soluzioni e fare clic sull'icona di configurazione di ASP.NET o passare ai menu del sito Web o del progetto e selezionare la voce di menu configurazione ASP.NET. Sfortunatamente, il WSAT può funzionare solo con i siti Web locali. Non è quindi possibile usare WSAT dalla workstation per gestire il sito Web nell'ambiente di produzione.

La novità è che tutte le funzionalità esposte da WSAT sono disponibili a livello di codice tramite le API di appartenenza e dei ruoli. Inoltre, molte delle schermate di WSAT usano i controlli standard correlati all'accesso ASP.NET. In breve, è possibile aggiungere pagine ASP.NET al sito Web che offrono le funzionalità di gestione necessarie.

Tenere presente che un'esercitazione precedente ha aggiornato l'applicazione Web Book revisioni per includere una cartella `~/Admin` e che questa cartella è stata configurata in modo da consentire solo agli utenti con il ruolo di amministratore. È stata aggiunta una pagina alla cartella denominata `CreateAccount.aspx` da cui un amministratore può creare un nuovo account utente. Questa pagina usa il controllo CreateUserWizard per visualizzare l'interfaccia utente e la logica di back-end per la creazione di un nuovo account utente. Inoltre, ho personalizzato il controllo per includere una casella di controllo che chiede se il nuovo utente debba essere aggiunto al ruolo di amministratore (vedere la figura 5). Con un po' di lavoro è possibile creare un set personalizzato di pagine che implementa le attività relative alla gestione di utenti e ruoli che verrebbero altrimenti fornite da WSAT.

> [!NOTE]
> Per altre informazioni sull'uso delle API di appartenenza e dei ruoli insieme ai controlli Web ASP.NET correlati all'accesso, vedere le [*esercitazioni sulla sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Per ulteriori informazioni sulla personalizzazione del controllo CreateUserWizard, vedere la pagina relativa alla [*creazione di account utente*](../../older-versions-security/membership/creating-user-accounts-vb.md) e all'archiviazione di ulteriori esercitazioni [*sulle informazioni utente*](../../older-versions-security/membership/storing-additional-user-information-vb.md) oppure vedere l'articolo di [*Erich Peterson*](http://www.erichpeterson.com/) , [*personalizzazione del controllo CreateUserWizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).

[![gli amministratori possono creare nuovi account utente](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figura 5**: gli amministratori possono creare nuovi account utente ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Se sono necessarie le funzionalità complete di WSAT, vedere la [*pagina relativa allo strumento di amministrazione del sito Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), in cui l'autore Dan Clem illustra il processo di creazione di uno strumento personalizzato di tipo WSAT. Dan condivide il codice sorgente dell'applicazione (in C#) e fornisce istruzioni dettagliate per aggiungerlo al sito Web ospitato.

## <a name="summary"></a>Riepilogo

Quando si distribuisce un'applicazione Web che utilizza l'implementazione del database dei servizi dell'applicazione, è necessario innanzitutto verificare che il database di produzione disponga degli oggetti di database necessari. Questi oggetti possono essere aggiunti usando le tecniche descritte nell'esercitazione *distribuzione di un database* . in alternativa, è possibile usare lo strumento `aspnet_regsql.exe`, come illustrato in questa esercitazione. Altri problemi sono stati affrontati al centro attorno alla sincronizzazione del nome dell'applicazione usato negli ambienti di sviluppo e produzione (importante se si desidera che gli utenti e i ruoli creati nell'ambiente di sviluppo siano validi per la produzione) e le tecniche per gestione degli utenti e dei ruoli nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [*Strumento di registrazione SQL Server ASP.NET (aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Creazione del database di Servizi per le applicazioni per SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Creazione dello schema di appartenenza in SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Esaminare l'appartenenza, i ruoli e il profilo di ASP.NET*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Implementazione dello strumento di amministrazione del sito Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Esercitazioni sulla sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Panoramica dello strumento Amministrazione sito Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Precedente](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Successivo](strategies-for-database-development-and-deployment-vb.md)
