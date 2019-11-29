---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Utenti e ruoli nel sito Web di produzioneC#() | Microsoft Docs
author: rick-anderson
description: Lo strumento di amministrazione del sito Web ASP.NET (WSAT) fornisce un'interfaccia utente basata sul Web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, la modifica di...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: c47bd2c1661f129dd8856916de04b8ba459fbfec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616629"
---
# <a name="users-and-roles-on-the-production-website-c"></a>Utenti e ruoli nel sito Web di produzioneC#()

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Lo strumento di amministrazione del sito Web ASP.NET (WSAT) fornisce un'interfaccia utente basata sul Web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, la modifica e l'eliminazione di utenti e ruoli. Sfortunatamente, il WSAT funziona solo se visitato da localhost, ovvero non è possibile raggiungere lo strumento di amministrazione del sito Web di produzione tramite il browser. La novità è che esistono soluzioni alternative che consentono di gestire gli utenti e i ruoli per la produzione. Questa esercitazione esamina queste soluzioni alternative e altre.

## <a name="introduction"></a>Introduzione

In ASP.NET 2,0 sono stati introdotti numerosi *servizi applicativi*, ovvero una suite di servizi di blocco predefiniti che è possibile aggiungere all'applicazione Web. I servizi appartenenza e ruoli sono stati aggiunti al sito Web revisioni del libro nell' [esercitazione *configurazione di un sito Web che usa servizi per le applicazioni* ](configuring-a-website-that-uses-application-services-cs.md). Il servizio di appartenenza facilita la creazione e la gestione degli account utente; il servizio ruoli offre un'API per la categorizzazione degli utenti in gruppi. Il sito revisioni del libro ha tre account utente: Scott, Jisun e Alice e un singolo ruolo, admin, con Scott e Jisun nel ruolo di amministratore.

ASP. I servizi delle applicazioni di rete non sono collegati a un'implementazione specifica. Al contrario, si indica ai servizi dell'applicazione di utilizzare un determinato *provider*e tale provider implementa il servizio utilizzando una tecnologia particolare. È stata configurata l'applicazione Web Book revisioni per usare i provider `SqlMembershipProvider` e `SqlRoleProvider` per i servizi di appartenenza e ruoli. Questi due provider archiviano le informazioni sugli account utente e sui ruoli in un database di SQL Server e sono i provider più comunemente utilizzati per le applicazioni Web basate su Internet ospitate in una società di hosting Web.

Un problema comune per gli sviluppatori che utilizzano i servizi appartenenza e ruoli è la gestione degli utenti e dei ruoli nell'ambiente di produzione. Come si elimina un account utente dal sito Web di produzione, si aggiunge un nuovo ruolo o si aggiunge un utente esistente a un ruolo esistente? Questa esercitazione illustra diverse tecniche per la gestione di utenti e ruoli nel sito Web di produzione.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Utilizzo dello strumento Amministrazione sito Web di ASP.NET

ASP.NET include uno [strumento di amministrazione del sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) che semplifica la creazione e la gestione di account utente e ruoli e per specificare le regole di autorizzazione basate su utenti e ruoli. Per usare WSAT, fare clic sull'icona di configurazione di ASP.NET nel Esplora soluzioni o andare al sito Web o al menu progetto e scegliere l'opzione di configurazione ASP.NET. Entrambi gli approcci avviano un Web browser e lo indirizzano a WSAT a un indirizzo come: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Il WSAT è suddiviso in tre sezioni:

- **Sicurezza** : gestisce utenti, ruoli e regole di autorizzazione.
- **ApplicationConfiguration** -Gestisci le impostazioni &lt;appSettings&gt; e SMTP da qui. È anche possibile portare l'applicazione offline e gestire le impostazioni di debug e traccia da qui, nonché specificare la pagina di errore personalizzata predefinita.
- **ProviderConfiguration** -configura i provider usati dai servizi dell'applicazione.

La sezione sicurezza (mostrata nella **Figura 1**) include i collegamenti per la creazione di nuovi utenti, la gestione degli utenti, la creazione e la gestione dei ruoli e la creazione e la gestione delle regole di accesso. Da qui è possibile aggiungere un nuovo ruolo al sistema, eliminare un utente esistente o aggiungere o rimuovere ruoli da un account utente specifico.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: la sezione sicurezza di WSAT include le opzioni per la gestione di utenti e ruoli  
([Fare clic per visualizzare l'immagine con dimensioni complete](users-and-roles-on-the-production-website-cs/_static/image3.png))

Sfortunatamente, WSAT è accessibile solo localmente. Non è possibile visitare il WSAT nel sito Web di produzione remoto; Se si visita `www.yoursite.com/asp.netwebadminfiles/default.aspx` si ottiene una risposta 404 non trovata. Il codice che consente di WSAT usa le classi `Membership` e `Roles` nel .NET Framework per creare, modificare ed eliminare utenti e ruoli. Queste classi consultano le informazioni di configurazione dell'applicazione Web per determinare il provider da usare. Tornando all'esercitazione [ *configurazione di un sito Web che usa servizi per le applicazioni viene* ](configuring-a-website-that-uses-application-services-cs.md) configurato il sito Web revisioni del libro per l'uso dei provider `SqlMembershipProvider` e `SqlRoleProvider`. Questa operazione ha richiesto l'aggiunta di `<membership>` e `<roleManager>` sezioni per `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Si noti che le sezioni `<membership>` e `<roleManager>` fanno riferimento rispettivamente ai provider di `SqlMembershipProvider` e `SqlRoleProvider` nell'attributo `type`. Questi provider archiviano le informazioni relative a utenti e ruoli in un database di SQL Server specificato. Il database utilizzato da tali provider viene specificato dall'attributo `connectionStringName`, `ReviewsConnectionString`, definito nel file `~/ConfigSections/databaseConnectionStrings.config`. Ricordare che il file `databaseConnectionStrings.config` nell'ambiente di sviluppo contiene la stringa di connessione al database di sviluppo, mentre il file di `databaseConnectionStrings.config` in produzione contiene la stringa di connessione al database di produzione.

In breve, il WSAT deve essere accessibile localmente tramite l'ambiente di sviluppo e funziona con le informazioni relative a utenti e ruoli nel database specificato nel file di `databaseConnectionStrings.config`. Di conseguenza, se si modificano le informazioni sulla stringa di connessione nel file di `databaseConnectionStrings.config` nell'ambiente di sviluppo, è possibile usare WSAT localmente per gestire utenti e ruoli nell'ambiente di produzione.

Per illustrare questa funzionalità, aprire il file di `databaseConnectionStrings.config` in Visual Studio nell'ambiente di sviluppo e sostituire la stringa di connessione del database di sviluppo con la stringa di connessione del database di produzione. Avviare quindi il WSAT, passare alla scheda sicurezza e aggiungere un nuovo utente denominato Sam con password "password!". (meno le virgolette). **Nella figura 2** viene visualizzata la schermata WSAT durante la creazione dell'account.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: creare un nuovo utente denominato Sam nell'ambiente di produzione  
([Fare clic per visualizzare l'immagine con dimensioni complete](users-and-roles-on-the-production-website-cs/_static/image6.png))

Poiché è stata modificata la stringa di connessione in `databaseConnectionStrings.config` in modo che punti al server di database di produzione, Sam è stato aggiunto come utente nell'ambiente di produzione. Per verificarlo, modificare la stringa di connessione nel file di `databaseConnectionStrings.config` di nuovo nel database di sviluppo, quindi visitare la pagina `Login.aspx` nell'ambiente di sviluppo. Provare ad accedere come Sam (vedere la **Figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: non è possibile accedere come Sam nell'ambiente di sviluppo  
([Fare clic per visualizzare l'immagine con dimensioni complete](users-and-roles-on-the-production-website-cs/_static/image9.png))

Non è possibile accedere come Sam nell'ambiente di sviluppo perché le informazioni sull'account utente non esistono nel database locale. Al contrario, è stato aggiunto al database di produzione. Per verificarlo, visualizzare il contenuto della tabella `aspnet_Users` nei database di sviluppo e di produzione. Nell'ambiente di sviluppo devono essere presenti solo tre record per gli utenti Scott, Jisun e Alice. Tuttavia, nella tabella `aspnet_Users` del database di produzione sono presenti quattro record: Scott, Jisun, Alice e Sam. Di conseguenza, Sam può accedere attraverso il sito Web in produzione, ma non tramite l'ambiente di sviluppo.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam può accedere al sito Web di produzione  
([Fare clic per visualizzare l'immagine con dimensioni complete](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Non dimenticare di modificare la stringa di connessione nel file di `databaseConnectionStrings.config` alla stringa di connessione del database di sviluppo al termine dell'uso di WSAT. in caso contrario, si lavoreranno con i dati di produzione durante il test del sito tramite l'ambiente di sviluppo. Tenere inoltre presente che, mentre la tecnica appena illustrata consente di usare WSAT per gestire in remoto gli utenti e i ruoli, le modifiche apportate alle altre opzioni di configurazione di WSAT (regole di accesso, impostazioni SMTP, impostazioni di debug e di traccia e così via) modificano il file di `Web.config`. Di conseguenza, tutte le modifiche apportate alle impostazioni si applicano all'ambiente di sviluppo e non all'ambiente di produzione.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Creazione di pagine Web personalizzate per la gestione di utenti e ruoli

WSAT fornisce un sistema predefinito per la gestione di utenti e ruoli, ma può essere avviato solo localmente e richiede modifiche alle informazioni della stringa di connessione per poter gestire gli utenti e i ruoli in produzione. La maggior parte dei siti Web che supportano gli account utente include anche alcune pagine Web di amministrazione di utenti e ruoli che consentono agli amministratori di gestire gli utenti e i ruoli da pagine all'interno del sito. Tali pagine di amministrazione basate sul Web semplificano notevolmente la gestione di utenti e ruoli e sono essenziali per i siti in cui potrebbero essere presenti molti amministratori o amministratori che non hanno accesso o lo sfondo tecnico per l'uso di Visual Studio per avviare il WSAT.

ASP.NET include una serie di controlli Web correlati all'accesso incorporati che rendono l'implementazione di molte di queste pagine Web amministrative con la semplice funzionalità di trascinamento della selezione. Ad esempio, è possibile creare una pagina per consentire agli amministratori di creare un nuovo account utente trascinando il controllo CreateUserWizard nella pagina e impostando alcune proprietà. Infatti, la pagina per la creazione di utenti in WSAT illustrata nella **Figura 2** usa lo stesso controllo CreateUserWizard che è possibile aggiungere alle pagine. Inoltre, le funzionalità dei servizi di appartenenza e di ruoli sono disponibili a livello di programmazione tramite le classi `Membership` e `Roles` nel .NET Framework. Con queste classi è possibile scrivere codice per creare, modificare ed eliminare utenti e ruoli, nonché per aggiungere o rimuovere utenti ai ruoli, per determinare quali utenti sono i ruoli e per eseguire altre attività correlate a utenti e ruoli.

Nell'esercitazione [ *configurazione di un sito Web che usa servizi per le applicazioni* ](configuring-a-website-that-uses-application-services-cs.md) ho aggiunto una pagina alla cartella `Admin` denominata `CreateAccount.aspx`. Questa pagina consente a un amministratore di aggiungere un nuovo account utente al sito e di specificare se l'utente appena creato si trova nel ruolo di amministratore (vedere la **Figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: gli amministratori possono creare nuovi account utente  
([Fare clic per visualizzare l'immagine con dimensioni complete](users-and-roles-on-the-production-website-cs/_static/image15.png))

Per informazioni più dettagliate sulla creazione di pagine di amministrazione di utenti e ruoli, oltre a istruzioni dettagliate sull'uso delle classi `Membership` e `Roles` e dei controlli Web ASP.NET correlati all'accesso, vedere le [esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Sono disponibili indicazioni su come creare pagine Web per la creazione di nuovi account, la creazione e la gestione di ruoli, l'assegnazione di utenti ai ruoli e altre attività amministrative comuni.

Per implementare la funzionalità simile a WSAT nel sito Web di produzione, è sempre possibile creare una serie personalizzata di pagine Web che implementano le funzionalità di WSAT. Per iniziare, vedere il codice sorgente di WSAT, che si trova nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Un'altra opzione consiste nell'usare l'alternativa WSAT di Dan Clem, che condivide nel suo articolo, in cui viene implementato [lo strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan esamina i lettori attraverso il processo di creazione di uno strumento di tipo WSAT personalizzato, include il codice sorgente dell'applicazione per il C#download (in) e fornisce istruzioni dettagliate per l'aggiunta di WSAT personalizzati a un sito Web ospitato.

## <a name="summary"></a>Riepilogo

È possibile usare lo strumento Amministrazione sito Web di ASP.NET (WSAT) insieme ai servizi delle applicazioni per l'appartenenza e i ruoli per gestire le informazioni relative a utenti e ruoli per il sito Web. Sfortunatamente, WSAT è accessibile solo in locale e non può essere visitato dal sito Web di produzione. Tuttavia, modificando la stringa di connessione nell'ambiente di sviluppo in modo che punti al database di produzione, è possibile usare WSAT per gestire gli utenti e i ruoli nel sito Web di produzione.

Sebbene l'approccio WSAT offra un metodo rapido e semplice per gestire utenti e ruoli, richiede l'avvio di WSAT da Visual Studio, oltre a modifiche temporanee alle informazioni della stringa di connessione. Il WSAT offre un modo rapido per gestire utenti e ruoli in produzione, ma è complesso e non funziona correttamente per i siti Web con più amministratori o con amministratori che non hanno o non hanno familiarità con Visual Studio e WSAT. Per questi motivi, la maggior parte dei siti Web che supportano gli account utente include un set di pagine Web amministrative. Un set di pagine Web di questo tipo Elimina la necessità di WSAT e viene usata da diversi utenti amministratori da qualsiasi computer.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Esame di ASP. Appartenenza, ruoli e profilo di NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementazione dello strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Panoramica dello strumento Amministrazione sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](precompiling-your-website-cs.md)
> [Successivo](asp-net-hosting-options-vb.md)
