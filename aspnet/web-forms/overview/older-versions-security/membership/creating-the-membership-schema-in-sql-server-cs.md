---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Creazione dello schema di appartenenza in SQL ServerC#() | Microsoft Docs
author: rick-anderson
description: Questa esercitazione inizia con l'analisi delle tecniche per l'aggiunta dello schema necessario al database per poter utilizzare l'oggetto SqlMembershipProvider. In seguito, Wi...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 97623e7c13ab7799b9dadbb8e52be8e0cd99e252
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575334"
---
# <a name="creating-the-membership-schema-in-sql-server-c"></a>Creazione dello schema di appartenenza in SQL Server (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Questa esercitazione inizia con l'analisi delle tecniche per l'aggiunta dello schema necessario al database per poter utilizzare l'oggetto SqlMembershipProvider. In seguito, verranno esaminate le tabelle principali nello schema e ne verranno illustrate le finalità e l'importanza. Questa esercitazione termina con un'occhiata a come indicare a un'applicazione ASP.NET il provider che il Framework di appartenenza deve usare.

## <a name="introduction"></a>Introduzione

Le due esercitazioni precedenti hanno esaminato l'uso dell'autenticazione basata su form per identificare i visitatori del sito Web Il Framework di autenticazione basata su form consente agli sviluppatori di registrare con facilità un utente in un sito Web e di ricordarli nelle visite di pagina tramite l'utilizzo dei ticket di autenticazione. La classe `FormsAuthentication` include metodi per generare il ticket e aggiungerlo ai cookie del visitatore. Il `FormsAuthenticationModule` esamina tutte le richieste in ingresso e, per quelle con un ticket di autenticazione valido, crea e associa una `GenericPrincipal` e un oggetto `FormsIdentity` con la richiesta corrente. L'autenticazione basata su form è semplicemente un meccanismo per concedere un ticket di autenticazione a un visitatore quando si esegue l'accesso e, in caso di richieste successive, l'analisi del ticket per determinare l'identità dell'utente. Affinché un'applicazione Web supporti gli account utente, è comunque necessario implementare un archivio utenti e aggiungere funzionalità per convalidare le credenziali, registrare nuovi utenti e la miriade di altre attività relative agli account utente.

Prima di ASP.NET 2,0, gli sviluppatori dovevano implementare tutte le attività relative agli account utente. Fortunatamente il team ASP.NET ha riconosciuto questa lacuna e ha introdotto il framework delle appartenenze con ASP.NET 2,0. Il Framework di appartenenza è un set di classi nella .NET Framework che forniscono un'interfaccia a livello di codice per l'esecuzione di attività di base relative all'account utente. Questo Framework è basato sul [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di inserire un'implementazione personalizzata in un'API standardizzata.

Come illustrato nell'esercitazione <a id="Tutorial1"> </a> [*nozioni di base sulla sicurezza e supporto per ASP.NET*](../introduction/security-basics-and-asp-net-support-cs.md) , il .NET Framework viene fornito con due provider di appartenenze predefiniti: [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) e [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Come suggerisce il nome, il `SqlMembershipProvider` usa un database Microsoft SQL Server come archivio utente. Per usare questo provider in un'applicazione, è necessario indicare al provider il database da usare come archivio. Come si può immaginare, il `SqlMembershipProvider` prevede che il database dell'archivio utenti disponga di determinate tabelle, viste e stored procedure di database. È necessario aggiungere questo schema previsto al database selezionato.

Questa esercitazione inizia con l'analisi delle tecniche per l'aggiunta dello schema necessario al database per poter utilizzare il `SqlMembershipProvider`. In seguito, verranno esaminate le tabelle principali nello schema e ne verranno illustrate le finalità e l'importanza. Questa esercitazione termina con un'occhiata a come indicare a un'applicazione ASP.NET il provider che il Framework di appartenenza deve usare.

Ecco come procedere.

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Passaggio 1: decidere dove posizionare l'archivio utente

I dati di un'applicazione ASP.NET vengono in genere archiviati in un numero di tabelle in un database. Quando si implementa lo schema del database di `SqlMembershipProvider`, è necessario decidere se inserire lo schema di appartenenza nello stesso database dei dati dell'applicazione o in un database alternativo.

Si consiglia di individuare lo schema di appartenenza nello stesso database dei dati dell'applicazione per i motivi seguenti:

- **Gestione** di un'applicazione i cui dati sono incapsulati in un database sono più facili da comprendere, gestire e distribuire rispetto a un'applicazione che dispone di due database distinti.
- **Integrità relazionale** individuando le tabelle correlate all'appartenenza nello stesso database delle tabelle dell'applicazione è possibile stabilire [vincoli di chiave esterna](http://en.wikipedia.org/wiki/Foreign_key) tra le chiavi primarie nelle tabelle correlate all'appartenenza e nelle tabelle delle applicazioni correlate.

Separare l'archivio utenti e i dati dell'applicazione in database distinti è opportuno solo se si dispone di più applicazioni che utilizzano database distinti, ma che devono condividere un archivio utente comune.

### <a name="creating-a-database"></a>Creazione di un database

L'applicazione compilata poiché la seconda esercitazione non ha ancora richiesto un database. Ne abbiamo bisogno, tuttavia, per l'archivio utenti. È possibile crearne uno e quindi aggiungervi lo schema richiesto dal provider `SqlMembershipProvider` (vedere il passaggio 2).

> [!NOTE]
> In questa serie di esercitazioni si utilizzerà un database [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) per archiviare le tabelle dell'applicazione e lo schema di `SqlMembershipProvider`. Questa decisione è stata presa per due motivi: per prima cosa, a causa del costo gratuito, l'edizione Express è la versione più accessibile di SQL Server 2005; in secondo luogo, i database di SQL Server 2005 Express Edition possono essere inseriti direttamente nella cartella `App_Data` dell'applicazione Web, rendendolo un pacchetto cinch per comprimere il database e l'applicazione Web in un file ZIP e per ridistribuirlo senza alcuna istruzione di configurazione speciale o opzioni di configurazione. Se si preferisce continuare a usare una versione non Express Edition di SQL Server, è possibile farlo gratuitamente. I passaggi sono praticamente identici. Lo schema `SqlMembershipProvider` funzionerà con qualsiasi versione di Microsoft SQL Server 2000.

Dal Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella `App_Data` e scegliere Aggiungi nuovo elemento. Se nel progetto non viene visualizzata una cartella `App_Data`, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere Aggiungi cartella ASP.NET e selezionare `App_Data`.) Nella finestra di dialogo Aggiungi nuovo elemento scegliere di aggiungere un nuovo database SQL denominato `SecurityTutorials.mdf`. In questa esercitazione si aggiungerà lo schema di `SqlMembershipProvider` al database. nelle esercitazioni successive si creeranno tabelle aggiuntive per acquisire i dati dell'applicazione.

[![aggiungere un nuovo database SQL denominato SecurityTutorials. MDF alla cartella App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: aggiungere un nuovo database SQL denominato `SecurityTutorials.mdf` database alla cartella `App_Data` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))

L'aggiunta di un database alla cartella `App_Data` lo include automaticamente nella visualizzazione Esplora database. Nella versione non Express Edition di Visual Studio, il Esplora database viene chiamato Esplora server. Passare alla Esplora database ed espandere il database `SecurityTutorials` appena aggiunto. Se non viene visualizzata la Esplora database sullo schermo, andare al menu Visualizza e scegliere Esplora database oppure premere CTRL + ALT + S. Come illustrato nella figura 2, il database `SecurityTutorials` è vuoto, non contiene tabelle, viste e stored procedure.

[![il database SecurityTutorials è attualmente vuoto](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: il database `SecurityTutorials` è attualmente vuoto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Passaggio 2: aggiunta dello schema di`SqlMembershipProvider`al database

Il `SqlMembershipProvider` richiede l'installazione di un particolare set di tabelle, viste e stored procedure nel database dell'archivio utenti. Questi oggetti di database necessari possono essere aggiunti utilizzando lo [strumento`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). Questo file si trova nella cartella `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`.

> [!NOTE]
> Lo strumento `aspnet_regsql.exe` offre funzionalità della riga di comando e un'interfaccia utente grafica. L'interfaccia grafica è più intuitiva ed è quella che verrà esaminata in questa esercitazione. L'interfaccia della riga di comando è utile quando l'aggiunta dello schema di `SqlMembershipProvider` deve essere automatizzata, ad esempio negli script di compilazione o negli scenari di test automatizzati.

Lo strumento `aspnet_regsql.exe` viene usato per aggiungere o rimuovere i *servizi delle applicazioni ASP.NET* in un database di SQL Server specificato. I servizi dell'applicazione ASP.NET includono gli schemi per la `SqlMembershipProvider` e `SqlRoleProvider`, insieme agli schemi per i provider basati su SQL per altri framework ASP.NET 2,0. Per lo strumento `aspnet_regsql.exe` è necessario fornire due informazioni:

- Se si desidera aggiungere o rimuovere servizi applicativi e
- Database da cui aggiungere o rimuovere lo schema dei servizi dell'applicazione

Nella richiesta del database da utilizzare, lo strumento `aspnet_regsql.exe` richiede di fornire il nome del server in cui si trova il database, le credenziali di sicurezza per la connessione al database e il nome del database. Se si utilizza l'edizione non Express di SQL Server, è necessario che queste informazioni siano già note, perché si tratta delle stesse informazioni che è necessario fornire tramite una stringa di connessione quando si utilizza il database tramite una pagina Web ASP.NET. Determinare il nome del server e del database quando si utilizza un database SQL Server 2005 Express Edition nella cartella `App_Data`, tuttavia, è un po' più impegnativo.

Nella sezione seguente viene esaminato un modo semplice per specificare il nome del server e del database per un database di SQL Server 2005 Express Edition nella cartella `App_Data`. Se non si usa SQL Server 2005 Express Edition è possibile passare direttamente alla sezione installazione del Servizi per le applicazioni.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Determinazione del nome del server e del database per un database di SQL Server 2005 Express Edition nella cartella`App_Data`

Per utilizzare lo strumento `aspnet_regsql.exe` è necessario che i nomi del server e del database siano riconoscibili. Il nome del server è `localhost\InstanceName`. Probabilmente, *NomeIstanza* è `SQLExpress`. Tuttavia, se è stato installato SQL Server 2005 Express Edition manualmente, ovvero non è stato installato automaticamente durante l'installazione di Visual Studio, è possibile selezionare un nome di istanza diverso.

Il nome del database è un po' più complicato da determinare. I database nella cartella `App_Data` in genere hanno un nome di database che include un [identificatore univoco globale](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) insieme al percorso del file di database. È necessario determinare il nome del database per aggiungere lo schema dei servizi dell'applicazione tramite `aspnet_regsql.exe`.

Il modo più semplice per verificare il nome del database consiste nell'esaminarlo tramite SQL Server Management Studio. SQL Server Management Studio offre un'interfaccia grafica per la gestione dei database SQL Server 2005, ma non con l'edizione Express di SQL Server 2005. La novità è che è [possibile scaricare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) l'edizione Express gratuita di SQL Server Management Studio.

> [!NOTE]
> Se sul desktop è installata anche una versione non Express Edition di SQL Server 2005, è probabile che sia installata la versione completa di Management Studio. È possibile utilizzare la versione completa per determinare il nome del database, seguendo la stessa procedura descritta di seguito per l'edizione Express.

Per iniziare, chiudere Visual Studio per assicurarsi che tutti i blocchi imposti da Visual Studio nel file di database siano chiusi. Avviare quindi SQL Server Management Studio e connettersi al database `localhost\InstanceName` per SQL Server 2005 Express Edition. Come indicato in precedenza, è probabile che il nome dell'istanza sia `SQLExpress`. Per l'opzione autenticazione selezionare autenticazione di Windows.

[![connettersi all'istanza di SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: connettersi all'istanza di SQL Server 2005 Express Edition ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))

Dopo la connessione all'istanza di SQL Server 2005 Express Edition, Management Studio Visualizza le cartelle per i database, le impostazioni di sicurezza, gli oggetti server e così via. Se si espande la scheda database, si noterà che il database di `SecurityTutorials.mdf` *non* è registrato nell'istanza del database. è necessario innanzitutto alleghi il database.

Fare clic con il pulsante destro del mouse sulla cartella database e scegliere Connetti dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Connetti database. Da qui, fare clic sul pulsante Aggiungi, passare al database di `SecurityTutorials.mdf` e fare clic su OK. Nella figura 4 viene illustrata la finestra di dialogo Connetti database dopo che è stato selezionato il database `SecurityTutorials.mdf`. Nella figura 5 viene illustrata la Esplora oggetti di Management Studio dopo che il database è stato collegato correttamente.

[![collegato il database SecurityTutorials. MDF](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: allineare il Database[di `SecurityTutorials.mdf` (fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))

[![il database SecurityTutorials. MDF viene visualizzato nella cartella database](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: il database `SecurityTutorials.mdf` viene visualizzato nella cartella database ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))

Come illustrato nella figura 5, il database `SecurityTutorials.mdf` ha un nome piuttosto astruso. Modificare il nome in modo più memorabile (e più facile da digitare). Fare clic con il pulsante destro del mouse sul database, scegliere Rinomina dal menu di scelta rapida e rinominarlo `SecurityTutorialsDatabase`. Il nome del file non viene modificato, ma solo il nome utilizzato dal database per identificarsi SQL Server.

[![rinominare il database in SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: rinominare il Database in `SecurityTutorialsDatabase`([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))

A questo punto si conoscono i nomi del server e del database per il file di database `SecurityTutorials.mdf`: `localhost\InstanceName` e `SecurityTutorialsDatabase`, rispettivamente. A questo punto è possibile installare i servizi dell'applicazione tramite lo strumento `aspnet_regsql.exe`.

### <a name="installing-the-application-services"></a>Installazione del Servizi per le applicazioni

Per avviare lo strumento `aspnet_regsql.exe`, andare al menu Start e scegliere Esegui. Immettere `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` nella casella di testo e fare clic su OK. In alternativa, è possibile utilizzare Esplora risorse per eseguire il drill-down nella cartella appropriata e fare doppio clic sul file `aspnet_regsql.exe`. Entrambi gli approcci utilizzeranno gli stessi risultati.

Eseguendo lo strumento `aspnet_regsql.exe` senza argomenti della riga di comando, viene avviata l'interfaccia utente grafica dell'installazione guidata di ASP.NET SQL Server. La procedura guidata consente di aggiungere o rimuovere facilmente i servizi delle applicazioni ASP.NET in un database specificato. La prima schermata della procedura guidata, illustrata nella figura 7, descrive lo scopo dello strumento.

[![usare l'installazione guidata di ASP.NET SQL Server consente di aggiungere lo schema di appartenenza](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: usare l'installazione guidata di ASP.NET SQL Server consente di aggiungere lo schema di appartenenza ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))

Il secondo passaggio della procedura guidata chiede se si desidera aggiungere o rimuovere i servizi dell'applicazione. Poiché si desidera aggiungere le tabelle, le viste e le stored procedure necessarie per la `SqlMembershipProvider`, scegliere l'opzione configura SQL Server per i servizi dell'applicazione. In seguito, se si desidera rimuovere questo schema dal database, eseguire nuovamente la procedura guidata, ma scegliere l'opzione Rimuovi informazioni servizi applicazioni da un database esistente.

[![scegliere l'opzione configura SQL Server per Servizi per le applicazioni](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: scegliere l'opzione configura SQL Server per servizi per le applicazioni ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))

Il terzo passaggio richiede le informazioni sul database, ovvero il nome del server, le informazioni di autenticazione e il nome del database. Se si è stati seguiti insieme a questa esercitazione e si è aggiunto il database di `SecurityTutorials.mdf` a `App_Data`, questo è stato associato a `localhost\InstanceName`e rinominato in `SecurityTutorialsDatabase`, utilizzare i valori seguenti:

- Server: `localhost\InstanceName`
- Autenticazione di Windows
- Database: `SecurityTutorialsDatabase`

[![immettere le informazioni sul database](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: immettere le informazioni sul database ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))

Dopo aver immesso le informazioni sul database, fare clic su Avanti. Il passaggio finale riepiloga i passaggi che verranno eseguiti. Fare clic su Avanti per installare i servizi dell'applicazione, quindi completare la procedura guidata.

> [!NOTE]
> Se è stato utilizzato Management Studio per allegare il database e rinominare il file di database, assicurarsi di scollegare il database e chiudere Management Studio prima di riaprire Visual Studio. Per scollegare il database di `SecurityTutorialsDatabase`, fare clic con il pulsante destro del mouse sul nome del database e scegliere Disconnetti dal menu attività.

Al termine della procedura guidata, tornare a Visual Studio e passare alla Esplora database. Espandere la cartella Tabelle. Verrà visualizzata una serie di tabelle i cui nomi iniziano con il prefisso `aspnet_`. Analogamente, è possibile trovare una vasta gamma di viste e stored procedure nelle cartelle viste e stored procedure. Questi oggetti di database costituiscono lo schema dei servizi dell'applicazione. Nel passaggio 3 vengono esaminati gli oggetti di database appartenenti e specifici del ruolo.

[![un'ampia gamma di tabelle, viste e stored procedure sono state aggiunte al database](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: nel database sono state aggiunte diverse tabelle, viste e stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))

> [!NOTE]
> L'interfaccia utente grafica dello strumento `aspnet_regsql.exe` installa l'intero schema dei servizi dell'applicazione. Tuttavia, quando si esegue `aspnet_regsql.exe` dalla riga di comando, è possibile specificare quali componenti specifici dei servizi dell'applicazione installare o rimuovere. Se pertanto si desidera aggiungere solo le tabelle, le viste e le stored procedure necessarie per i provider `SqlMembershipProvider` e `SqlRoleProvider`, eseguire `aspnet_regsql.exe` dalla riga di comando. In alternativa, è possibile eseguire manualmente il subset appropriato di script di creazione T-SQL usati da `aspnet_regsql.exe`. Questi script si trovano nella cartella `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` con nomi quali `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`e così via.

A questo punto sono stati creati gli oggetti di database necessari per il `SqlMembershipProvider`. Tuttavia, è comunque necessario istruire il Framework di appartenenza che deve usare il `SqlMembershipProvider` (rispetto a `ActiveDirectoryMembershipProvider`) e che il `SqlMembershipProvider` deve usare il database `SecurityTutorials`. Si esaminerà come specificare il provider da usare e come personalizzare le impostazioni del provider selezionato nel passaggio 4. Prima di tutto, è opportuno esaminare in modo approfondito gli oggetti di database appena creati.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Passaggio 3: esaminare le tabelle principali dello schema

Quando si utilizzano i Framework di appartenenza e ruoli in un'applicazione ASP.NET, i dettagli di implementazione vengono incapsulati dal provider. Nelle esercitazioni future si interfaccerà con questi Framework tramite le classi `Membership` e `Roles` di .NET Framework. Quando si usano queste API di alto livello, non è necessario preoccuparsi dei dettagli di basso livello, come le query eseguite o quali tabelle vengono modificate dal `SqlMembershipProvider` e `SqlRoleProvider`.

A questo punto, è possibile usare i Framework di appartenenza e ruoli senza avere esplorato lo schema del database creato nel passaggio 2. Tuttavia, quando si creano le tabelle per archiviare i dati dell'applicazione, potrebbe essere necessario creare entità correlate a utenti o ruoli. Consente di acquisire familiarità con gli schemi `SqlMembershipProvider` e `SqlRoleProvider` quando si stabiliscono vincoli di chiave esterna tra le tabelle di dati dell'applicazione e le tabelle create nel passaggio 2. In alcuni casi rari, inoltre, potrebbe essere necessario interfacciarsi con gli archivi di utenti e ruoli direttamente a livello di database, anziché tramite le classi `Membership` o `Roles`.

### <a name="partitioning-the-user-store-into-applications"></a>Partizionamento dell'archivio utente nelle applicazioni

I Framework di appartenenza e di ruoli sono progettati in modo tale che un singolo utente e un archivio di ruoli possano essere condivisi tra molte applicazioni diverse. Un'applicazione ASP.NET che usa i Framework appartenenza o ruoli deve specificare quale partizione applicativa usare. In breve, più applicazioni Web possono utilizzare gli stessi archivi di utenti e ruoli. La figura 11 illustra gli archivi di utenti e ruoli partizionati in tre applicazioni: HRSite, CustomerSite e SalesSite. Ognuna di queste tre applicazioni Web dispone di utenti e ruoli univoci, ma tutte le informazioni relative all'account utente e al ruolo vengono archiviate fisicamente nelle stesse tabelle di database.

[![gli account utente possono essere partizionati in più applicazioni](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: gli account utente possono essere partizionati in più applicazioni ([fare clic per visualizzare l'immagine con dimensioni complete](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))

La tabella `aspnet_Applications` definisce le partizioni. Ogni applicazione che utilizza il database per archiviare le informazioni sull'account utente è rappresentata da una riga di questa tabella. La tabella `aspnet_Applications` dispone di quattro colonne: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`e `Description`. `ApplicationId` è di tipo [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) e è la chiave primaria della tabella. `ApplicationName` fornisce un nome descrittivo univoco per ogni applicazione.

Le altre tabelle relative alle appartenenze e ai ruoli sono riportate al campo `ApplicationId` in `aspnet_Applications`. Ad esempio, la tabella `aspnet_Users`, che contiene un record per ogni account utente, dispone di un campo di chiave esterna `ApplicationId`; Ditto per la tabella `aspnet_Roles`. Il campo `ApplicationId` di queste tabelle specifica la partizione dell'applicazione a cui appartiene l'account utente o il ruolo.

### <a name="storing-user-account-information"></a>Archiviazione delle informazioni sull'account utente

Le informazioni sull'account utente sono ospitate in due tabelle: `aspnet_Users` e `aspnet_Membership`. La tabella `aspnet_Users` contiene i campi che contengono le informazioni essenziali sull'account utente. Le tre colonne più pertinenti sono:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` è la chiave primaria e di tipo `uniqueidentifier`. `UserName` è di tipo `nvarchar(256)` e, insieme alla password, costituisce le credenziali dell'utente. La password di un utente viene archiviata nella tabella `aspnet_Membership`. `ApplicationId` collega l'account utente a una particolare applicazione nel `aspnet_Applications`. È presente un [vincolo di`UNIQUE`](https://msdn.microsoft.com/library/ms191166.aspx) composito nelle colonne `UserName` e `ApplicationId`. Ciò garantisce che in una determinata applicazione ogni nome utente sia univoco, ma consente di usare lo stesso `UserName` in applicazioni diverse.

La tabella `aspnet_Membership` include informazioni aggiuntive sull'account utente, ad esempio la password dell'utente, l'indirizzo di posta elettronica, la data e l'ora dell'ultimo accesso e così via. Esiste una corrispondenza uno-a-uno tra i record nelle tabelle `aspnet_Users` e `aspnet_Membership`. Questa relazione è garantita dal campo `UserId` in `aspnet_Membership`, che funge da chiave primaria della tabella. Analogamente alla tabella `aspnet_Users`, `aspnet_Membership` include un campo `ApplicationId` che associa queste informazioni a una determinata partizione di applicazione.

### <a name="securing-passwords"></a>Protezione delle password

Le informazioni sulla password vengono archiviate nella tabella `aspnet_Membership`. Il `SqlMembershipProvider` consente l'archiviazione delle password nel database utilizzando una delle tre tecniche seguenti:

- **Clear** : la password viene archiviata nel database come testo normale. Si sconsiglia vivamente di utilizzare questa opzione. Se il database è compromesso, si tratta di un pirata informatico che trova un back-door o un dipendente scontento con accesso al database. le credenziali di ogni singolo utente sono disponibili per l'acquisizione.
- **Hashed** : le password vengono sottoposto a hashing usando un algoritmo hash unidirezionale e un valore salt generato in modo casuale. Questo valore con hash (insieme al Salt) viene archiviato nel database.
- **Encrypted** : una versione crittografata della password viene archiviata nel database.

La tecnica di archiviazione delle password utilizzata dipende dalle impostazioni `SqlMembershipProvider` specificate in `Web.config`. Si esaminerà la personalizzazione delle impostazioni `SqlMembershipProvider` nel passaggio 4. Il comportamento predefinito consiste nell'archiviare l'hash della password.

Le colonne responsabili dell'archiviazione della password sono `Password`, `PasswordFormat`e `PasswordSalt`. `PasswordFormat` è un campo di tipo `int` il cui valore indica la tecnica utilizzata per archiviare la password: 0 per Clear; 1 per Hashed; 2 per crittografato. a `PasswordSalt` viene assegnata una stringa generata in modo casuale indipendentemente dalla tecnica di archiviazione delle password utilizzata; il valore di `PasswordSalt` viene usato solo per il calcolo dell'hash della password. Infine, nella colonna `Password` sono contenuti i dati effettivi della password, ovvero la password in testo normale, l'hash della password o la password crittografata.

Nella tabella 1 sono illustrate le tre colonne che potrebbero essere simili per le varie tecniche di archiviazione durante l'archiviazione del segreto della password. .

| **Tecnica di archiviazione&lt;\_O3A\_p/&gt;** | **Password&lt;\_O3A\_p/&gt;** | **PasswordFormat&lt;\_O3A\_p/&gt;** | **PasswordSalt&lt;\_O3A\_p/&gt;** |
| --- | --- | --- | --- |
| Cancella | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Crittografato | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabella 1**: valori di esempio per i campi relativi alla password durante l'archiviazione del segreto della password.

> [!NOTE]
> La crittografia o l'algoritmo hash specifico utilizzato dal `SqlMembershipProvider` è determinato dalle impostazioni nell'elemento `<machineKey>`. Questo elemento di configurazione è stato illustrato nel passaggio 3 <a id="Tutorial3"> </a>dell'esercitazione [*configurazione dell'autenticazione basata su form e argomenti avanzati*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) .

### <a name="storing-roles-and-role-associations"></a>Archiviazione di ruoli e associazioni di ruolo

Il Framework dei ruoli consente agli sviluppatori di definire un set di ruoli e di specificare quali utenti appartengono ai ruoli. Queste informazioni vengono acquisite nel database tramite due tabelle: `aspnet_Roles` e `aspnet_UsersInRoles`. Ogni record nella tabella `aspnet_Roles` rappresenta un ruolo per una particolare applicazione. Analogamente alla tabella `aspnet_Users`, nella tabella `aspnet_Roles` sono disponibili tre colonne attinenti alla nostra discussione:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` è la chiave primaria e di tipo `uniqueidentifier`. `RoleName` è di tipo `nvarchar(256)`. E `ApplicationId` collega l'account utente a una particolare applicazione in `aspnet_Applications`. È presente un vincolo di `UNIQUE` composito nelle colonne `RoleName` e `ApplicationId`, assicurando che in una determinata applicazione ogni nome di ruolo sia univoco.

La tabella `aspnet_UsersInRoles` funge da mapping tra utenti e ruoli. Sono disponibili solo due colonne, `UserId` e `RoleId`, che costituiscono una chiave primaria composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Passaggio 4: specifica del provider e personalizzazione delle impostazioni

Tutti i Framework che supportano il modello di provider, ad esempio i Framework di appartenenza e di ruoli, non hanno i dettagli di implementazione e delegano invece tale responsabilità a una classe del provider. Nel caso del Framework di appartenenza, la classe `Membership` definisce l'API per la gestione degli account utente, ma non interagisce direttamente con nessun archivio utente. Al contrario, i metodi della classe `Membership` consegnano la richiesta al provider configurato. verrà usato il `SqlMembershipProvider`. Quando si richiama uno dei metodi nella classe `Membership`, in che modo il Framework di appartenenza SA delegare la chiamata al `SqlMembershipProvider`?

La classe `Membership` dispone di una [proprietà`Providers`](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) che contiene un riferimento a tutte le classi di provider registrate disponibili per l'utilizzo da parte del framework delle appartenenze. A ogni provider registrato sono associati un nome e un tipo. Il nome offre un modo semplice per fare riferimento a un particolare provider nella raccolta di `Providers`, mentre il tipo identifica la classe del provider. Ogni provider registrato può inoltre includere le impostazioni di configurazione. Le impostazioni di configurazione per il Framework di appartenenza includono `passwordFormat` e `requiresUniqueEmail`, tra le molte altre. Vedere la tabella 2 per un elenco completo delle impostazioni di configurazione usate dal `SqlMembershipProvider`.

Il contenuto della proprietà `Providers` viene specificato tramite le impostazioni di configurazione dell'applicazione Web. Per impostazione predefinita, tutte le applicazioni Web dispongono di un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`. Il provider di appartenenze predefinito è registrato nel `machine.config` (disponibile in `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Come illustrato nel markup precedente, l' [elemento`<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx) definisce le impostazioni di configurazione per il Framework di appartenenza mentre l' [elemento figlio`<providers>`](https://msdn.microsoft.com/library/6d4936ht.aspx) specifica i provider registrati. I provider possono essere aggiunti o rimossi usando gli elementi [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) o [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) . utilizzare l'elemento [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) per rimuovere tutti i provider attualmente registrati. Come illustrato nel markup precedente, `machine.config` aggiunge un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`.

Oltre agli attributi `name` e `type`, l'elemento `<add>` contiene attributi che definiscono i valori per le varie impostazioni di configurazione. Nella tabella 2 sono elencate le impostazioni di configurazione disponibili per `SqlMembershipProvider`specifiche, oltre a una descrizione di ciascuna di esse.

> [!NOTE]
> Tutti i valori predefiniti indicati nella tabella 2 fanno riferimento ai valori predefiniti definiti nella classe `SqlMembershipProvider`. Si noti che non tutte le impostazioni di configurazione in `AspNetSqlMembershipProvider` corrispondono ai valori predefiniti della classe `SqlMembershipProvider`. Se, ad esempio, non è specificato in un provider di appartenenze, l'impostazione predefinita del `requiresUniqueEmail` è true. Tuttavia, il `AspNetSqlMembershipProvider` esegue l'override di questo valore predefinito specificando in modo esplicito il valore `false`.

| **Impostazione&lt;\_O3A\_p/&gt;** | **Descrizione&lt;\_O3A\_p/&gt;** |
| --- | --- |
| `ApplicationName` | Tenere presente che il Framework di appartenenza consente di partizionare un singolo archivio utente tra più applicazioni. Questa impostazione indica il nome della partizione di applicazione utilizzata dal provider di appartenenze. Se questo valore non viene specificato in modo esplicito, viene impostato in fase di esecuzione sul valore del percorso della radice virtuale dell'applicazione. |
| `commandTimeout` | Specifica il valore di timeout del comando SQL (in secondi). Il valore predefinito è 30. |
| `connectionStringName` | Nome della stringa di connessione nell'elemento `<connectionStrings>` da utilizzare per la connessione al database dell'archivio utenti. Questo valore è obbligatorio. |
| `description` | Fornisce una descrizione intuitiva del provider registrato. |
| `enablePasswordRetrieval` | Specifica se gli utenti possono recuperare la password dimenticata. Il valore predefinito è `false`. |
| `enablePasswordReset` | Indica se gli utenti possono reimpostare la password. Il valore predefinito è `true`. |
| `maxInvalidPasswordAttempts` | Numero massimo di tentativi di accesso non riusciti che possono verificarsi per un determinato utente durante il `passwordAttemptWindow` specificato prima che l'utente venga bloccato. Il valore predefinito è 5. |
| `minRequiredNonalphanumericCharacters` | Numero minimo di caratteri non alfanumerici che devono essere visualizzati nella password di un utente. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 1. |
| `minRequiredPasswordLength` | Numero minimo di caratteri richiesti in una password. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 7. |
| `name` | Nome del provider registrato. Questo valore è obbligatorio. |
| `passwordAttemptWindow` | Numero di minuti durante i quali vengono rilevati i tentativi di accesso non riusciti. Se un utente fornisce credenziali di accesso non valide `maxInvalidPasswordAttempts` volte in questa finestra specificata, verranno bloccate. Il valore predefinito è 10. |
| `PasswordFormat` | Formato di archiviazione delle password: `Clear`, `Hashed`o `Encrypted`. Il valore predefinito è `Hashed`. |
| `passwordStrengthRegularExpression` | Se specificato, questa espressione regolare viene utilizzata per valutare l'attendibilità della password selezionata dall'utente durante la creazione di un nuovo account o la modifica della password. Il valore predefinito è una stringa vuota. |
| `requiresQuestionAndAnswer` | Specifica se un utente deve rispondere alla domanda di sicurezza durante il recupero o la reimpostazione della password. Il valore predefinito è `true`. |
| `requiresUniqueEmail` | Indica se tutti gli account utente in una determinata partizione applicativa devono disporre di un indirizzo di posta elettronica univoco. Il valore predefinito è `true`. |
| `type` | Specifica il tipo del provider. Questo valore è obbligatorio. |

**Tabella 2**: impostazioni di appartenenza e di configurazione `SqlMembershipProvider`

Oltre ai `AspNetSqlMembershipProvider`, altri provider di appartenenze possono essere registrati in base all'applicazione mediante l'aggiunta di markup analogo al file di `Web.config`.

> [!NOTE]
> Il Framework dei ruoli funziona allo stesso modo: è presente un provider di ruoli registrato predefinito in `machine.config` e i provider registrati possono essere personalizzati in base all'applicazione in `Web.config`. In un'esercitazione futura si esamineranno in dettaglio il Framework dei ruoli e il markup di configurazione.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizzazione delle impostazioni di`SqlMembershipProvider`

Il `SqlMembershipProvider` predefinito (`AspNetSqlMembershipProvider`) dispone dell'attributo `connectionStringName` impostato su `LocalSqlServer`. Come il provider di `AspNetSqlMembershipProvider`, il nome della stringa di connessione `LocalSqlServer` è definito in `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Come si può notare, questa stringa di connessione definisce un database SQL 2005 Express Edition disponibile in | DataDirectory | aspnetdb. MDF. Stringa | DataDirectory | viene convertito in fase di esecuzione in modo che punti alla directory `~/App_Data/`, quindi il percorso del database | DataDirectory | aspnetdb. MDF viene convertito in `~/App_Data`/`aspnet.mdf`.

Se non sono state specificate informazioni sul provider di appartenenze nel file di `Web.config` dell'applicazione, l'applicazione usa il provider di appartenenze registrato predefinito, `AspNetSqlMembershipProvider`. Se il database `~/App_Data/aspnet.mdf` non esiste, il runtime di ASP.NET lo creerà automaticamente e aggiungerà lo schema dei servizi dell'applicazione. Tuttavia, non si vuole usare il database di `aspnet.mdf`; si vuole invece usare il database `SecurityTutorials.mdf` creato nel passaggio 2. Questa modifica può essere eseguita in uno dei due modi seguenti:

- <strong>Specificare un valore per il nome della</strong> stringa di connessione<strong>`LocalSqlServer`</strong> <strong>in</strong> <strong>`Web.config`</strong> <strong>.</strong> Sovrascrivendo il valore del nome della stringa di connessione `LocalSqlServer` in `Web.config`, è possibile utilizzare il provider di appartenenze registrato predefinito (`AspNetSqlMembershipProvider`) e utilizzarlo correttamente con il database `SecurityTutorials.mdf`. Questo approccio è corretto se si è soddisfatti delle impostazioni di configurazione specificate da `AspNetSqlMembershipProvider`. Per altre informazioni su questa tecnica, vedere il post di Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/)relativo [alla configurazione di ASP.NET 2,0 servizi per le applicazioni per l'uso di SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Aggiungere un nuovo provider registrato di tipo</strong> <strong>`SqlMembershipProvider`</strong> <strong>e configurarne</strong> l'impostazione<strong>`connectionStringName`</strong> <strong>in modo che punti al</strong> database<strong>`SecurityTutorials.mdf`</strong> <strong>.</strong> Questo approccio è utile negli scenari in cui si desidera personalizzare altre proprietà di configurazione oltre alla stringa di connessione del database. Nei miei progetti utilizzerò sempre questo approccio grazie alla sua flessibilità e leggibilità.

Prima di poter aggiungere un nuovo provider registrato che fa riferimento al database di `SecurityTutorials.mdf`, è necessario prima aggiungere un valore della stringa di connessione appropriato nella sezione `<connectionStrings>` in `Web.config`. Il markup seguente aggiunge una nuova stringa di connessione denominata `SecurityTutorialsConnectionString` che fa riferimento al database SQL Server 2005 Express Edition `SecurityTutorials.mdf` nella cartella `App_Data`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Se si utilizza un file di database alternativo, aggiornare la stringa di connessione in base alle esigenze. Per ulteriori informazioni sulla forma della stringa di connessione corretta, fare riferimento a [connectionStrings.com](http://www.connectionstrings.com/).

Aggiungere quindi il markup di configurazione di appartenenza seguente al file di `Web.config`. Questo markup registra un nuovo provider denominato `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Oltre a registrare il provider di `SecurityTutorialsSqlMembershipProvider`, il markup precedente definisce la `SecurityTutorialsSqlMembershipProvider` come provider predefinito (tramite l'attributo `defaultProvider` nell'elemento `<membership>`). Ricordare che il Framework di appartenenza può avere più provider registrati. Poiché `AspNetSqlMembershipProvider` viene registrato come primo provider in `machine.config`, funge da provider predefinito a meno che non sia indicato diversamente.

Attualmente, l'applicazione ha due provider registrati: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. Tuttavia, prima di registrare il provider di `SecurityTutorialsSqlMembershipProvider`, avremmo potuto cancellare tutti i provider precedentemente registrati aggiungendo un [elemento`<clear />`](https://msdn.microsoft.com/library/t062y6yc.aspx) immediatamente prima dell'elemento `<add>`. In questo modo si cancellano i `AspNetSqlMembershipProvider` dall'elenco dei provider registrati, il che significa che il `SecurityTutorialsSqlMembershipProvider` sarebbe l'unico provider di appartenenze registrato. Se è stato usato questo approccio, non è necessario contrassegnare il `SecurityTutorialsSqlMembershipProvider` come provider predefinito, perché sarebbe l'unico provider di appartenenze registrato. Per ulteriori informazioni sull'utilizzo di `<clear />`, vedere [utilizzo di `<clear />` durante l'aggiunta di provider](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Si noti che l'impostazione di `connectionStringName` del `SecurityTutorialsSqlMembershipProvider`fa riferimento al nome della stringa di connessione `SecurityTutorialsConnectionString` appena aggiunto e che la relativa impostazione `applicationName` è stata impostata sul valore SecurityTutorials. Inoltre, l'impostazione `requiresUniqueEmail` è stata impostata su `true`. Tutte le altre opzioni di configurazione sono identiche ai valori in `AspNetSqlMembershipProvider`. Se lo si desidera, è possibile apportare qualsiasi modifica alla configurazione. Ad esempio, è possibile limitare la complessità della password richiedendo due caratteri non alfanumerici anziché uno oppure aumentando la lunghezza della password a otto caratteri invece di sette.

> [!NOTE]
> Tenere presente che il Framework di appartenenza consente di partizionare un singolo archivio utente tra più applicazioni. L'impostazione `applicationName` del provider di appartenenza indica l'applicazione utilizzata dal provider quando si utilizza l'archivio utente. È importante impostare in modo esplicito un valore per l'impostazione di configurazione `applicationName` perché se il `applicationName` non è impostato in modo esplicito, viene assegnato al percorso radice virtuale dell'applicazione Web in fase di esecuzione. Questa operazione funziona correttamente purché il percorso radice virtuale dell'applicazione non cambi, ma se si sposta l'applicazione in un percorso diverso, anche l'impostazione `applicationName` cambierà. Quando si verifica questo problema, il provider di appartenenze inizierà a lavorare con una partizione di applicazione diversa da quella usata in precedenza. Gli account utente creati prima dello spostamento si troveranno in una partizione di applicazione diversa e tali utenti non saranno più in grado di accedere al sito. Per una discussione più approfondita su questo aspetto, vedere [impostare sempre la proprietà `applicationName` quando si configura l'appartenenza a ASP.NET 2,0 e altri provider](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Riepilogo

A questo punto è presente un database con i servizi delle applicazioni configurati (`SecurityTutorials.mdf`) e l'applicazione Web è stata configurata in modo che il Framework di appartenenza usi il provider di `SecurityTutorialsSqlMembershipProvider` appena registrato. Questo provider registrato è di tipo `SqlMembershipProvider` e il relativo `connectionStringName` impostato sulla stringa di connessione appropriata (`SecurityTutorialsConnectionString`) e sul valore di `applicationName` impostato in modo esplicito.

A questo punto è possibile usare il framework delle appartenenze dell'applicazione. Nell'esercitazione successiva verrà esaminato come creare nuovi account utente. In seguito si esaminerà l'autenticazione degli utenti, l'esecuzione dell'autorizzazione basata sull'utente e l'archiviazione di informazioni aggiuntive relative all'utente nel database.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Impostare sempre la proprietà `applicationName` quando si configura l'appartenenza a ASP.NET 2,0 e altri provider](https://weblogs.asp.net/scottgu/443634)
- [Configurazione di ASP.NET 2,0 Servizi per le applicazioni per l'uso di SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Scarica SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Esame dell'appartenenza, dei ruoli e del profilo di ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Elemento `<add>` per i provider per l'appartenenza](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Elemento `<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Elemento `<providers>` per l'appartenenza](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Utilizzo di `<clear />` durante l'aggiunta di provider](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Lavorare direttamente con la `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione video sugli argomenti contenuti in questa esercitazione

- [Informazioni sulle appartenenze ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurazione di SQL per l'uso degli schemi di appartenenza](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modifica delle impostazioni di appartenenza nello schema di appartenenza predefinito](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è Alicja Maziarz. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [avanti](creating-user-accounts-cs.md)
