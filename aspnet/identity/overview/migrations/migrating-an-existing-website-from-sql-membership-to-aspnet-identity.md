---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrazione di un sito Web esistente dall'appartenenza SQL a ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: Questa esercitazione illustra i passaggi per eseguire la migrazione di un'applicazione Web esistente con i dati utente e ruolo creati usando l'appartenenza a SQL alla nuova ASP.NET Identity s...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 633229cc4311d151121bf6a91b9fa8aeecca1197
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583727"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity

di [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> Questa esercitazione illustra i passaggi per eseguire la migrazione di un'applicazione Web esistente con i dati utente e ruolo creati usando l'appartenenza a SQL al nuovo sistema di ASP.NET Identity. Questo approccio comporta la modifica dello schema del database esistente in quello necessario per il ASP.NET Identity e l'hook nelle classi precedenti/nuove. Dopo aver adottato questo approccio, una volta eseguita la migrazione del database, gli aggiornamenti futuri dell'identità verranno gestiti senza intoppi.

Per questa esercitazione verrà creato un modello di applicazione Web (Web Form) con Visual Studio 2010 per creare dati di utenti e ruoli. Si utilizzeranno quindi gli script SQL per eseguire la migrazione del database esistente alle tabelle richieste dal sistema di identità. Verranno quindi installati i pacchetti NuGet necessari e verranno aggiunte nuove pagine di gestione degli account che usano il sistema di identità per la gestione delle appartenenze. Come test della migrazione, gli utenti creati con l'appartenenza a SQL dovrebbero essere in grado di accedere e i nuovi utenti devono essere in grado di effettuare la registrazione. L'esempio completo è disponibile [qui](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Vedere anche [migrazione dall'appartenenza a ASP.NET a ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Per iniziare

### <a name="creating-an-application-with-sql-membership"></a>Creazione di un'applicazione con appartenenza a SQL

1. È necessario iniziare con un'applicazione esistente che usa l'appartenenza a SQL e disponga di dati utente e ruolo. Ai fini di questo articolo, è ora di creare un'applicazione Web in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Utilizzando lo strumento di configurazione ASP.NET, creare due utenti: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Creare un ruolo denominato admin e aggiungere "oldAdminUser" come utente in tale ruolo.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Creare una sezione di amministrazione del sito con un. aspx predefinito. Impostare il tag di autorizzazione nel file Web. config per consentire l'accesso solo agli utenti nei ruoli di amministratore. Altre informazioni sono disponibili qui [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Visualizzare il database in Esplora server per comprendere le tabelle create dal sistema di appartenenze SQL. I dati di accesso utente vengono archiviati nelle tabelle ASPNET\_Users e ASPNET\_Membership, mentre i dati del ruolo vengono archiviati nella tabella ASPNET\_Roles. Informazioni sugli utenti in cui vengono archiviati i ruoli nella tabella ASPNET\_UsersInRoles. Per la gestione delle appartenenze di base è sufficiente trasferire le informazioni nelle tabelle precedenti al sistema di ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrazione a Visual Studio 2013

1. Installare Visual Studio Express 2013 per il Web o Visual Studio 2013 insieme agli [aggiornamenti più recenti](https://www.microsoft.com/download/details.aspx?id=44921).
2. Aprire il progetto precedente nella versione installata di Visual Studio. Se SQL Server Express non è installato nel computer, viene visualizzato un messaggio di richiesta quando si apre il progetto, perché la stringa di connessione usa SQL Express. È possibile scegliere di installare SQL Express o come soluzione alternativa modificare la stringa di connessione nel database locale. Per questo articolo verrà modificato nel database locale.
3. Aprire Web. config e modificare la stringa di connessione da. SQLEXPRESS a (local DB) v 11.0. Rimuovere ' User Instance = true ' dalla stringa di connessione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Aprire Esplora server e verificare che i dati e lo schema della tabella possano essere osservati.
5. Il sistema ASP.NET Identity funziona con la versione 4,5 o una versione successiva del Framework. Ridestinare l'applicazione a 4,5 o versione successiva.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compilare il progetto per verificare che non siano presenti errori.

### <a name="installing-the-nuget-packages"></a>Installazione dei pacchetti NuGet

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto &gt; **Gestisci pacchetti NuGet**. Nella casella di ricerca immettere "Asp.net Identity". Selezionare il pacchetto nell'elenco dei risultati e fare clic su Installa. Accettare il contratto di licenza facendo clic sul pulsante "Accetto". Si noti che questo pacchetto installerà i pacchetti di dipendenza: EntityFramework e Microsoft ASP.NET Identity core. Analogamente, installare i pacchetti seguenti (ignorare gli ultimi 4 pacchetti OWIN se non si vuole abilitare l'accesso OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrare il database nel nuovo sistema di identità

Il passaggio successivo consiste nel migrare il database esistente a uno schema richiesto dal sistema di ASP.NET Identity. A tale scopo, viene eseguito uno script SQL con un set di comandi per creare nuove tabelle ed eseguire la migrazione delle informazioni utente esistenti alle nuove tabelle. Il file di script è disponibile [qui](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Questo file di script è specifico di questo esempio. Se lo schema per le tabelle create con l'appartenenza a SQL è personalizzato o modificato, gli script devono essere modificati di conseguenza.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Come generare lo script SQL per la migrazione dello schema

Per ASP.NET Identity classi da utilizzare con i dati degli utenti esistenti, è necessario eseguire la migrazione dello schema del database a quello necessario per ASP.NET Identity. A tale scopo, è possibile aggiungere nuove tabelle e copiare le informazioni esistenti in tali tabelle. Per impostazione predefinita ASP.NET Identity utilizza EntityFramework per eseguire il mapping delle classi del modello di identità al database per archiviare/recuperare le informazioni. Queste classi modello implementano le interfacce di identità principali che definiscono gli oggetti utente e ruolo. Le tabelle e le colonne del database sono basate su queste classi di modelli. Le classi del modello EntityFramework in Identity v 2.1.0 e le relative proprietà sono definite di seguito

| **IdentityUser** | **Tipo** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | string | Id | RoleId | Provider | Id |
| Nome utente | string | NOME | UserId | UserId | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | ID\_utente |
| Email | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

È necessario disporre di tabelle per ognuno di questi modelli con colonne corrispondenti alle proprietà. Il mapping tra classi e tabelle viene definito nel metodo `OnModelCreating` della `IdentityDBContext`. Questa operazione è nota come metodo API Fluent per la configurazione e altre informazioni sono disponibili [qui](https://msdn.microsoft.com/data/jj591617.aspx). La configurazione per le classi è come indicato di seguito

| **Classe** | **Tabella** | **Chiave primaria** | **Chiave esterna** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | ID\_utente-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Provider + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | ID\_utente-&gt;AspnetUsers |

Con queste informazioni è possibile creare istruzioni SQL per la creazione di nuove tabelle. È possibile scrivere ogni singola istruzione o generare l'intero script usando i comandi di PowerShell di EntityFramework, che possono essere modificati in base alle esigenze. A tale scopo, in Visual Studio aprire la **console di gestione pacchetti** dal menu **Visualizza** o **strumenti** .

- Eseguire il comando "Enable-Migrations" per abilitare le migrazioni EntityFramework.
- Eseguire il comando "Add-Migration Initial", che crea il codice di installazione iniziale per creare C#il database in/VB.
- Il passaggio finale consiste nell'eseguire il comando "Update-database-script" che genera lo script SQL basato sulle classi del modello.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Questo script di generazione del database può essere usato come inizio per apportare ulteriori modifiche per aggiungere nuove colonne e copiare i dati. Il vantaggio è che viene generata la tabella `_MigrationHistory` utilizzata da EntityFramework per modificare lo schema del database quando le classi del modello cambiano per le versioni future delle versioni di identità.

Le informazioni sugli utenti di appartenenza a SQL hanno altre proprietà, oltre a quelle nella classe del modello utente di identità, ovvero posta elettronica, tentativi di password, data dell'ultimo accesso, data dell'ultimo blocco e così via. Si tratta di informazioni utili che possono essere trasferite al sistema di identità. Questa operazione può essere eseguita aggiungendo proprietà aggiuntive al modello utente e eseguendone il mapping alle colonne della tabella nel database. È possibile eseguire questa operazione aggiungendo una classe che esegue la sottoclasse del modello di `IdentityUser`. È possibile aggiungere le proprietà a questa classe personalizzata e modificare lo script SQL per aggiungere le colonne corrispondenti durante la creazione della tabella. Il codice per questa classe è descritto più avanti nell'articolo. Lo script SQL per la creazione della tabella `AspnetUsers` dopo l'aggiunta delle nuove proprietà sarà

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Successivamente, è necessario copiare le informazioni esistenti dal database di appartenenze SQL alle tabelle appena aggiunte per l'identità. Questa operazione può essere eseguita tramite SQL copiando i dati direttamente da una tabella all'altra. Per aggiungere dati nelle righe della tabella, si usa il costrutto `INSERT INTO [Table]`. Per eseguire la copia da un'altra tabella, è possibile usare l'istruzione `INSERT INTO` insieme all'istruzione `SELECT`. Per ottenere tutte le informazioni sugli utenti, è necessario eseguire una query sulle tabelle *aspnet\_Users* e *ASPNET\_Membership* e copiare i dati nella tabella *AspNetUsers* . Si usano il `INSERT INTO` e `SELECT` insieme alle istruzioni `JOIN` e `LEFT OUTER JOIN`. Per ulteriori informazioni sull'esecuzione di query e la copia di dati tra tabelle, fare riferimento a [questo](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) collegamento. Inoltre, le tabelle AspnetUserLogins e AspnetUserClaims sono vuote per iniziare poiché non sono presenti informazioni nell'appartenenza a SQL che vengono mappate a questo per impostazione predefinita. Le uniche informazioni copiate sono per utenti e ruoli. Per il progetto creato nei passaggi precedenti, la query SQL per la copia delle informazioni nella tabella users sarà

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Nell'istruzione SQL precedente, le informazioni su ogni utente delle tabelle *aspnet\_Users* e *ASPNET\_Membership* vengono copiate nelle colonne della tabella *AspnetUsers* . L'unica modifica eseguita qui è la copia della password. Poiché l'algoritmo di crittografia per le password nell'appartenenza a SQL USA ' PasswordSalt ' è PasswordFormat ', questo viene copiato insieme alla password con hash, in modo che possa essere usato per decrittografare la password in base all'identità. Questa operazione viene illustrata più avanti nell'articolo quando si associa un hash di password personalizzato.

Questo file di script è specifico di questo esempio. Per le applicazioni che dispongono di tabelle aggiuntive, gli sviluppatori possono seguire un approccio simile per aggiungere proprietà aggiuntive alla classe del modello utente ed eseguirne il mapping alle colonne nella tabella AspnetUsers. Per eseguire lo script,

1. Aprire Esplora server. Espandere la connessione ' ApplicationServices ' per visualizzare le tabelle. Fare clic con il pulsante destro del mouse sul nodo tabelle e selezionare l'opzione ' nuova query '

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Nella finestra query copiare e incollare l'intero script SQL dal file Migrations. SQL. Eseguire il file di script premendo il pulsante freccia "Esegui".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aggiornare la finestra di Esplora server. Nel database vengono create cinque nuove tabelle.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Di seguito viene illustrato il mapping delle informazioni nelle tabelle di appartenenza SQL al nuovo sistema di identità.

    Ruoli di\_ASPNET--&gt; AspNetRoles

    ASP\_netUsers e ASP\_netMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Come spiegato nella sezione precedente, le tabelle AspNetUserClaims e AspNetUserLogins sono vuote. Il campo ' discriminatore ' nella tabella AspNetUser deve corrispondere al nome della classe del modello definito come passaggio successivo. Inoltre, il formato della colonna PasswordHash è' password crittografata | sale password | formato password '. In questo modo è possibile usare la speciale logica di crittografia dell'appartenenza a SQL per poter riutilizzare le password obsolete. Questo argomento è illustrato più avanti in questo articolo.

### <a name="creating-models-and-membership-pages"></a>Creazione di modelli e pagine di appartenenza

Come indicato in precedenza, la funzionalità Identity USA Entity Framework per comunicare con il database per archiviare le informazioni sull'account per impostazione predefinita. Per lavorare con i dati esistenti nella tabella, è necessario creare classi di modello che eseguono il mapping alle tabelle e collegarle nel sistema di identità. Come parte del contratto di identità, le classi del modello devono implementare le interfacce definite nella dll Identity. core oppure possono estendere l'implementazione esistente di queste interfacce disponibili in Microsoft. AspNet. Identity. EntityFramework.

In questo esempio, le tabelle AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole includono colonne simili all'implementazione esistente del sistema di identità. Di conseguenza, è possibile riutilizzare le classi esistenti per eseguire il mapping a tali tabelle. La tabella AspNetUser include alcune colonne aggiuntive che vengono utilizzate per archiviare informazioni aggiuntive dalle tabelle di appartenenza SQL. Per eseguire il mapping è possibile creare una classe di modello che estenda l'implementazione esistente di ' IdentityUser ' e aggiungere le proprietà aggiuntive.

1. Creare una cartella Models nel progetto e aggiungere un utente della classe. Il nome della classe deve corrispondere ai dati aggiunti nella colonna ' Discriminator ' della tabella ' AspnetUsers '.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe utente deve estendere la classe IdentityUser trovata nella dll *Microsoft. AspNet. Identity. EntityFramework* . Dichiarare le proprietà nella classe di cui viene eseguito il mapping alle colonne AspNetUser. Le proprietà ID, username, PasswordHash e SecurityStamp sono definite nel IdentityUser e pertanto vengono omesse. Di seguito è riportato il codice per la classe utente che dispone di tutte le proprietà

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. È necessaria una classe Entity Framework DbContext per salvare in modo permanente i dati nei modelli nelle tabelle e recuperare i dati dalle tabelle per popolare i modelli. *Microsoft. AspNet. Identity. EntityFramework* dll definisce la classe IdentityDbContext che interagisce con le tabelle di identità per recuperare e archiviare le informazioni. Il IdentityDbContext&lt;TUser&gt; accetta una classe ' TUser ' che può essere una classe che estende la classe IdentityUser.

    Creare una nuova classe ApplicationDBContext che estende IdentityDbContext nella cartella "Models", passando la classe "User" creata nel passaggio 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. La gestione degli utenti nel nuovo sistema di identità viene eseguita usando la classe UserManager&lt;TUser&gt; definita nella dll *Microsoft. AspNet. Identity. EntityFramework* . È necessario creare una classe personalizzata che estende UserManager, passando la classe ' User ' creata nel passaggio 1.

    Nella cartella Models (modelli) creare una nuova classe UserManager che estende UserManager&lt;User&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Le password degli utenti dell'applicazione vengono crittografate e archiviate nel database. L'algoritmo di crittografia usato nell'appartenenza a SQL è diverso da quello nel nuovo sistema di identità. Per riutilizzare le password obsolete, è necessario decrittografare in modo selettivo le password quando gli utenti obsoleti accedono usando l'algoritmo di appartenenza a SQL, usando l'algoritmo di crittografia in Identity per i nuovi utenti.

    La classe UserManager ha una proprietà' PasswordHasher ' che archivia un'istanza di una classe che implementa l'interfaccia ' IPasswordHasher '. Viene utilizzato per crittografare/decrittografare le password durante le transazioni di autenticazione utente. Nella classe UserManager definita nel passaggio 3 creare una nuova classe SQLPasswordHasher e copiare il codice seguente.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Risolvere gli errori di compilazione importando gli spazi dei nomi System. Text e System. Security. Cryptography.

    Il metodo EncodePassword crittografa la password in base all'implementazione della crittografia di appartenenza SQL predefinita. Questa operazione viene eseguita dalla dll System. Web. Se l'app precedente usava un'implementazione personalizzata, questa dovrebbe essere riflessa qui. È necessario definire altri due metodi *HashPassword* e *VerifyHashedPassword* che usano il metodo *EncodePassword* per eseguire l'hashing di una determinata password o per verificare una password con testo normale con quella esistente nel database.

    Il sistema di appartenenze SQL usava PasswordHash, PasswordSalt e PasswordFormat per eseguire l'hashing della password immessa dagli utenti quando registrano o modificano la password. Durante la migrazione tutti i tre campi vengono archiviati nella colonna PasswordHash della tabella AspNetUser, separati dal carattere "|". Quando un utente esegue l'accesso e la password include questi campi, viene usata la crittografia di appartenenza SQL per controllare la password; in caso contrario, viene usata la crittografia predefinita del sistema di identità per verificare la password. In questo modo gli utenti obsoleti non avrebbero dovuto modificare le proprie password una volta eseguita la migrazione dell'app.
5. Dichiarare il costruttore per la classe UserManager e passarlo come SQLPasswordHasher alla proprietà nel costruttore.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crea nuove pagine di gestione degli account

Il passaggio successivo della migrazione consiste nell'aggiungere pagine di gestione degli account che consentono a un utente di eseguire la registrazione e l'accesso. Le pagine di account precedenti dell'appartenenza a SQL usano controlli che non funzionano con il nuovo sistema di identità. Per aggiungere le nuove pagine di gestione degli utenti, seguire l'esercitazione in questo collegamento [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) a partire dal passaggio "aggiunta di Web Form per la registrazione degli utenti all'applicazione" poiché il progetto è già stato creato e sono stati aggiunti i pacchetti NuGet.

È necessario apportare alcune modifiche affinché l'esempio funzioni con il progetto disponibile qui.

- Le classi code-behind Register.aspx.cs e Login.aspx.cs usano la `UserManager` dai pacchetti di identità per creare un utente. Per questo esempio usare UserManager aggiunto nella cartella Models attenendosi alla procedura descritta in precedenza.
- Usare la classe utente creata invece di IdentityUser nelle classi code-behind Register.aspx.cs e Login.aspx.cs. Questo hook nella classe utente personalizzata nel sistema di identità.
- La parte per la creazione del database può essere ignorata.
- Lo sviluppatore deve impostare ApplicationId per il nuovo utente in modo che corrisponda all'ID applicazione corrente. Questa operazione può essere eseguita eseguendo una query su ApplicationId per questa applicazione prima che un oggetto utente venga creato nella classe Register.aspx.cs e impostandolo prima della creazione dell'utente.

    Esempio:

    Definire un metodo nella pagina Register.aspx.cs per eseguire una query sulla tabella ASPNET\_Applications e ottenere l'ID applicazione in base al nome dell'applicazione

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    A questo punto, ottenere set this nell'oggetto User

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Usare il nome utente e la password precedenti per accedere a un utente esistente. Utilizzare la pagina Register per creare un nuovo utente. Verificare inoltre che gli utenti si trovino in ruoli come previsto.

Il porting al sistema di identità consente all'utente di aggiungere l'autenticazione di apertura (OAuth) all'applicazione. Vedere [l'esempio in](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) cui è abilitato OAuth.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato illustrato come trasferire gli utenti dall'appartenenza SQL a ASP.NET Identity, ma non sono stati portati i dati di profilo. Nell'esercitazione successiva si esamineranno i dati del profilo di trasferimento dall'appartenenza a SQL al nuovo sistema di identità.

È possibile lasciare commenti e suggerimenti nella parte inferiore di questo articolo.

*Grazie a Tom Dykstra e Rick Anderson per la revisione dell'articolo.*
