---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: ASP.NET Identity è un sistema estensibile che consente di creare un provider di archiviazione personalizzato e di inserirlo nell'applicazione senza rielaborare l'applicazione...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584413"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity

di [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity è un sistema estensibile che consente di creare un provider di archiviazione personalizzato e di inserirlo nell'applicazione senza riutilizzare l'applicazione. In questo argomento viene descritto come creare un provider di archiviazione personalizzato per ASP.NET Identity. Vengono illustrati i concetti importanti per la creazione di un provider di archiviazione personalizzato, ma non si tratta di una procedura dettagliata per l'implementazione di un provider di archiviazione personalizzato.
> 
> Per un esempio di implementazione di un provider di archiviazione personalizzato, vedere [implementazione di un provider di archiviazione MySQL ASP.NET Identity personalizzato](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Questo argomento è stato aggiornato per ASP.NET Identity 2,0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Visual Studio 2013 con Update 2
> - ASP.NET Identity 2

## <a name="introduction"></a>Introduzione

Per impostazione predefinita, il sistema ASP.NET Identity archivia le informazioni sugli utenti in un database SQL Server e USA Entity Framework Code First per creare il database. Per molte applicazioni, questo approccio funziona correttamente. Tuttavia, è preferibile usare un tipo diverso di meccanismo di persistenza, ad esempio l'archiviazione tabelle di Azure, oppure è possibile che si disponga già di tabelle di database con una struttura molto diversa rispetto all'implementazione predefinita. In entrambi i casi, è possibile scrivere un provider personalizzato per il meccanismo di archiviazione e collegare tale provider nell'applicazione.

ASP.NET Identity è incluso per impostazione predefinita in molti dei modelli di Visual Studio 2013. È possibile ottenere gli aggiornamenti ASP.NET Identity tramite il [pacchetto NuGet Microsoft ASPNET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Questo argomento include le sezioni seguenti:

- [Informazioni sull'architettura](#architecture)
- [Informazioni sui dati archiviati](#data)
- [Creare il livello di accesso ai dati](#dal)
- [Personalizzare la classe utente](#user)
- [Personalizzare l'archivio utenti](#userstore)
- [Personalizzare la classe Role](#role)
- [Personalizzare l'archivio dei ruoli](#rolestore)
- [Riconfigurare l'applicazione per l'uso di un nuovo provider di archiviazione](#reconfigure)
- [Altre implementazioni dei provider di archiviazione personalizzati](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Informazioni sull'architettura

ASP.NET Identity è costituito da classi denominate Manager e archivi. I responsabili sono classi generali che uno sviluppatore di applicazioni usa per eseguire operazioni, ad esempio la creazione di un utente, nel sistema di ASP.NET Identity. Gli archivi sono classi di livello inferiore che specificano il modo in cui le entità, ad esempio utenti e ruoli, vengono rese permanente. Gli archivi sono strettamente collegati al meccanismo di persistenza, ma i responsabili sono separati dai negozi, il che significa che è possibile sostituire il meccanismo di persistenza senza compromettere l'intera applicazione.

Il diagramma seguente illustra il modo in cui l'applicazione Web interagisce con i Manager e gli archivi interagiscono con il livello di accesso ai dati.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Per creare un provider di archiviazione personalizzato per ASP.NET Identity, è necessario creare l'origine dati, il livello di accesso ai dati e le classi di archivio che interagiscono con questo livello di accesso ai dati. È possibile continuare a utilizzare le stesse API di gestione per eseguire operazioni sui dati dell'utente, ma ora i dati vengono salvati in un sistema di archiviazione diverso.

Non è necessario personalizzare le classi Manager perché quando si crea una nuova istanza di UserManager o RoleManager si fornisce il tipo della classe utente e si passa un'istanza della classe Store come argomento. Questo approccio consente di collegare le classi personalizzate alla struttura esistente. Si vedrà come creare un'istanza di UserManager e RoleManager con le classi di archivio personalizzate nella sezione [riconfigurare l'applicazione per usare il nuovo provider di archiviazione](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Informazioni sui dati archiviati

Per implementare un provider di archiviazione personalizzato, è necessario comprendere i tipi di dati utilizzati con ASP.NET Identity e decidere quali funzionalità sono rilevanti per l'applicazione.

| Dati | Description |
| --- | --- |
| Utenti | Utenti registrati del sito Web. Include l'ID utente e il nome utente. Potrebbe includere una password con hash se gli utenti eseguono l'accesso con credenziali specifiche per il sito (invece di usare le credenziali di un sito esterno come Facebook) e l'indicatore di sicurezza per indicare se sono state apportate modifiche alle credenziali utente. Potrebbe includere anche l'indirizzo di posta elettronica, il numero di telefono, se è abilitata l'autenticazione a due fattori, il numero corrente di accessi non riusciti e se un account è stato bloccato. |
| Attestazioni utente | Set di istruzioni (o attestazioni) sull'utente che rappresenta l'identità dell'utente. Consente di abilitare un'espressione maggiore dell'identità dell'utente che può essere eseguita tramite i ruoli. |
| Account di accesso utente | Informazioni sul provider di autenticazione esterno, ad esempio Facebook, da usare per la registrazione in un utente. |
| Ruoli | Gruppi di autorizzazione per il sito. Include l'ID del ruolo e il nome del ruolo (ad esempio, "admin" o "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati

In questo argomento si presuppone che l'utente abbia familiarità con il meccanismo di persistenza che si intende usare e come creare entità per tale meccanismo. In questo argomento non vengono fornite informazioni dettagliate su come creare i repository o le classi di accesso ai dati. al contrario, vengono forniti alcuni suggerimenti sulle decisioni di progettazione che è necessario apportare quando si lavora con ASP.NET Identity.

Quando si progettano i repository per un provider di archiviazione personalizzato, è possibile avere una grande libertà. È sufficiente creare repository per le funzionalità che si intende usare nell'applicazione. Se, ad esempio, non si utilizzano ruoli nell'applicazione, non è necessario creare spazio di archiviazione per ruoli o ruoli utente. La tecnologia e l'infrastruttura esistente possono richiedere una struttura molto diversa dall'implementazione predefinita di ASP.NET Identity. Nel livello di accesso ai dati fornire la logica per lavorare con la struttura dei repository.

Per un'implementazione MySQL di repository di dati per ASP.NET Identity 2,0, vedere [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Nel livello di accesso ai dati fornire la logica per salvare i dati da ASP.NET Identity all'origine dati. Il livello di accesso ai dati per il provider di archiviazione personalizzato potrebbe includere le classi seguenti per archiviare le informazioni relative a utenti e ruoli.

| Classe | Description | Esempio |
| --- | --- | --- |
| Contesto | Incapsula le informazioni per la connessione al meccanismo di persistenza e l'esecuzione di query. Questa classe è fondamentale per il livello di accesso ai dati. Per le altre classi di dati è necessaria un'istanza di questa classe per eseguire le operazioni. Si inizializzano inoltre le classi dell'archivio con un'istanza di questa classe. | [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Archiviazione utente | Archivia e recupera le informazioni utente, ad esempio il nome utente e l'hash della password. | [UserTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Archiviazione del ruolo | Archivia e recupera le informazioni sui ruoli, ad esempio il nome del ruolo. | [RoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Archiviazione UserClaims | Archivia e recupera le informazioni sull'attestazione utente, ad esempio il tipo di attestazione e il valore. | [UserClaimsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Archiviazione UserLogins | Archivia e recupera le informazioni di accesso dell'utente, ad esempio un provider di autenticazione esterno. | [UserLoginsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Archiviazione UserRole | Archivia e recupera i ruoli assegnati a un utente. | [UserRoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Anche in questo caso, è necessario implementare solo le classi che si intende usare nell'applicazione.

Nelle classi di accesso ai dati si fornisce il codice per eseguire operazioni sui dati per il meccanismo di persistenza specifico. All'interno dell'implementazione di MySQL, ad esempio, la classe UserTable contiene un metodo per inserire un nuovo record nella tabella di database users. La variabile denominata `_database` è un'istanza della classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Dopo aver creato le classi di accesso ai dati, è necessario creare classi di archivio che chiamano i metodi specifici nel livello di accesso ai dati.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizzare la classe utente

Quando si implementa un provider di archiviazione personalizzato, è necessario creare una classe utente equivalente alla classe [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) nello spazio dei nomi [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Il diagramma seguente illustra la classe IdentityUser che è necessario creare e l'interfaccia da implementare in questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

L'interfaccia [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) definisce le proprietà che il usermanager tenta di chiamare durante l'esecuzione delle operazioni richieste. L'interfaccia contiene due proprietà-ID e UserName. L'interfaccia [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) consente di specificare il tipo di chiave per l'utente tramite il parametro generico **TKey** . Il tipo della proprietà ID corrisponde al valore del parametro TKey.

Il Framework di identità fornisce anche l'interfaccia [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) (senza il parametro generico) quando si vuole usare un valore stringa per la chiave.

La classe IdentityUser implementa IUser e contiene proprietà o costruttori aggiuntivi per gli utenti sul sito Web. Nell'esempio seguente viene illustrata una classe IdentityUser che usa un numero intero per la chiave. Il campo ID è impostato su **int** in modo che corrisponda al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Per un'implementazione completa, vedere [IdentityUser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizzare l'archivio utenti

Si crea anche una classe userStore ambito che fornisce i metodi per tutte le operazioni sui dati dell'utente. Questa classe è equivalente alla classe [userstore ambito&lt;TUser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) nello spazio dei nomi [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) . Nella classe userStore ambito si implementa il [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) e una qualsiasi delle interfacce facoltative. È possibile selezionare le interfacce facoltative da implementare in base alle funzionalità che si desidera fornire nell'applicazione.

Nell'immagine seguente viene illustrata la classe userStore ambito che è necessario creare e le interfacce pertinenti.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Il modello di progetto predefinito in Visual Studio contiene codice che presuppone che molte delle interfacce opzionali siano state implementate nell'archivio utente. Se si usa il modello predefinito con un archivio utente personalizzato, è necessario implementare le interfacce facoltative nell'archivio utente o modificare il codice del modello in modo che non chiami più metodi nelle interfacce non implementate.

 Nell'esempio seguente viene illustrata una semplice classe di archivio utenti. Il parametro generico **TUser** accetta il tipo della classe utente che in genere è la classe IdentityUser definita. Il parametro generico **TKey** accetta il tipo della chiave utente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In questo esempio, il costruttore che accetta un parametro denominato *database* di tipo ExampleDatabase è solo un esempio di come passare la classe di accesso ai dati. Ad esempio, nell'implementazione di MySQL, questo costruttore accetta un parametro di tipo MySQLDatabase. 

All'interno della classe userStore ambito, è possibile usare le classi di accesso ai dati create per eseguire operazioni. Nell'implementazione di MySQL, ad esempio, la classe userStore ambito ha il Metodo CreateAsync che usa un'istanza di UserTable per inserire un nuovo record. Il metodo **Insert** sull'oggetto **userTable** è lo stesso metodo illustrato nella sezione precedente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfacce da implementare quando si Personalizza l'archivio utenti

L'immagine successiva Mostra ulteriori dettagli sulle funzionalità definite in ogni interfaccia. Tutte le interfacce opzionali ereditano da IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  L'interfaccia [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) è l'unica interfaccia che è necessario implementare nell'archivio utente. Definisce i metodi per la creazione, l'aggiornamento, l'eliminazione e il recupero degli utenti.
- **IUserClaimStore**  
  L'interfaccia [IUserClaimStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) definisce i metodi che è necessario implementare nell'archivio utente per abilitare le attestazioni utente. Contiene metodi o aggiunta, rimozione e recupero di attestazioni utente.
- **IUserLoginStore**  
  Il [IUserLoginStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definisce i metodi che è necessario implementare nell'archivio utente per abilitare i provider di autenticazione esterni. Contiene i metodi per l'aggiunta, la rimozione e il recupero degli account di accesso utente e un metodo per il recupero di un utente in base alle informazioni di accesso.
- **IUserRoleStore**  
  L'interfaccia [IUserRoleStore&lt;TKey, TUser&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) definisce i metodi che è necessario implementare nell'archivio utente per eseguire il mapping di un utente a un ruolo. Contiene i metodi per aggiungere, rimuovere e recuperare i ruoli di un utente e un metodo per verificare se un utente è assegnato a un ruolo.
- **IUserPasswordStore**  
  L'interfaccia [IUserPasswordStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) definisce i metodi che è necessario implementare nell'archivio utente per rendere permanente le password con hash. Contiene i metodi per ottenere e impostare la password con hash e un metodo che indica se l'utente ha impostato una password.
- **IUserSecurityStampStore**  
  L'interfaccia [IUserSecurityStampStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) definisce i metodi che è necessario implementare nell'archivio utente per usare un indicatore di sicurezza per indicare se le informazioni sull'account dell'utente sono state modificate. Questo timbro viene aggiornato quando un utente modifica la password o aggiunge o rimuove gli account di accesso. Contiene i metodi per ottenere e impostare l'indicatore di sicurezza.
- **IUserTwoFactorStore**  
  L'interfaccia [IUserTwoFactorStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) definisce i metodi che è necessario implementare per implementare l'autenticazione a due fattori. Contiene i metodi per ottenere e impostare se per un utente è abilitata l'autenticazione a due fattori.
- **IUserPhoneNumberStore**  
  L'interfaccia [IUserPhoneNumberStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) definisce i metodi che è necessario implementare per archiviare i numeri di telefono dell'utente. Contiene i metodi per ottenere e impostare il numero di telefono e se il numero di telefono è confermato.
- **IUserEmailStore**  
  L'interfaccia [IUserEmailStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) definisce i metodi che è necessario implementare per archiviare gli indirizzi di posta elettronica dell'utente. Contiene i metodi per ottenere e impostare l'indirizzo di posta elettronica e verificare se il messaggio di posta elettronica è confermato.
- **IUserLockoutStore**  
  L'interfaccia [IUserLockoutStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) definisce i metodi che è necessario implementare per archiviare informazioni sul blocco di un account. Contiene i metodi per ottenere il numero corrente di tentativi di accesso non riusciti, ottenere e impostare se è possibile bloccare l'account, ottenere e impostare la data di fine blocco, incrementare il numero di tentativi non riusciti e reimpostare il numero di tentativi non riusciti.
- **IQueryableUserStore**  
  L'interfaccia [IQueryableUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) definisce i membri che è necessario implementare per fornire un archivio utenti Queryable. Contiene una proprietà che contiene gli utenti che eseguono query.

  Implementare le interfacce necessarie nell'applicazione. ad esempio, le interfacce IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore e IUserSecurityStampStore, come illustrato di seguito. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Per un'implementazione completa (incluse tutte le interfacce), vedere [userStore ambito (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin e IdentityUserRole

Lo spazio dei nomi Microsoft. AspNet. Identity. EntityFramework contiene le implementazioni delle classi [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)e [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Se si usano queste funzionalità, è possibile creare versioni personalizzate di queste classi e definire le proprietà per l'applicazione. Tuttavia, a volte è più efficiente non caricare queste entità in memoria durante l'esecuzione di operazioni di base, ad esempio l'aggiunta o la rimozione di un'attestazione dell'utente. Al contrario, le classi dell'archivio back-end possono eseguire queste operazioni direttamente sull'origine dati. Ad esempio, il metodo userStore ambito. GetClaimsAsync () può chiamare userClaimTable. FindByUserId (User. ID) per eseguire direttamente una query sulla tabella e restituire un elenco di attestazioni.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizzare la classe Role

Quando si implementa un provider di archiviazione personalizzato, è necessario creare una classe Role equivalente alla classe [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) nello spazio dei nomi [Microsoft. ASP. NET. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Il diagramma seguente illustra la classe IdentityRole che è necessario creare e l'interfaccia da implementare in questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

L'interfaccia [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) definisce le proprietà che il roleManager tenta di chiamare durante l'esecuzione delle operazioni richieste. L'interfaccia contiene due proprietà-ID e nome. L'interfaccia [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) consente di specificare il tipo di chiave per il ruolo tramite il parametro generico **TKey** . Il tipo della proprietà ID corrisponde al valore del parametro TKey.

Il Framework di identità fornisce anche l'interfaccia [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) (senza il parametro generico) quando si vuole usare un valore stringa per la chiave.

Nell'esempio seguente viene illustrata una classe IdentityRole che usa un numero intero per la chiave. Il campo ID è impostato su int in modo che corrisponda al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Per un'implementazione completa, vedere [IdentityRole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizzare l'archivio dei ruoli

Si crea anche una classe nell'rolestore che fornisce i metodi per tutte le operazioni di dati sui ruoli. Questa classe è equivalente alla classe [nell'rolestore&lt;TRole&gt;](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) nello spazio dei nomi Microsoft. ASP. NET. Identity. EntityFramework. Nella classe nell'rolestore si implementa [IRoleStore&lt;TRole, tkey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) e, facoltativamente, l'interfaccia [IQueryableRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) .

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Nell'esempio seguente viene illustrata una classe di archivio ruoli. Il parametro generico TRole accetta il tipo della classe Role che in genere è la classe IdentityRole definita. Il parametro generico TKey accetta il tipo della chiave Role. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  L'interfaccia [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) definisce i metodi da implementare nella classe dell'archivio dei ruoli. Contiene i metodi per la creazione, l'aggiornamento, l'eliminazione e il recupero di ruoli.
- **Nell'rolestore&lt;TRole&gt;**  
  Per personalizzare nell'rolestore, creare una classe che implementi l'interfaccia IRoleStore. È necessario implementare questa classe solo se si desidera utilizzare i ruoli nel sistema. Il costruttore che accetta un parametro denominato *database* di tipo ExampleDatabase è solo un esempio di come passare la classe di accesso ai dati. Ad esempio, nell'implementazione di MySQL, questo costruttore accetta un parametro di tipo MySQLDatabase.  
  
  Per un'implementazione completa, vedere [nell'rolestore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Riconfigurare l'applicazione per l'uso di un nuovo provider di archiviazione

È stato implementato il nuovo provider di archiviazione. A questo punto, è necessario configurare l'applicazione per l'uso di questo provider di archiviazione. Se il provider di archiviazione predefinito è stato incluso nel progetto, è necessario rimuovere il provider predefinito e sostituirlo con il provider.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Sostituisci provider di archiviazione predefinito nel progetto MVC

1. Nella finestra **Gestisci pacchetti NuGet** disinstallare il pacchetto **Microsoft ASP.NET Identity EntityFramework** . È possibile trovare questo pacchetto eseguendo una ricerca nei pacchetti installati per Identity. EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) verrà chiesto se si desidera disinstallare anche Entity Framework. Se non è necessario in altre parti dell'applicazione, è possibile disinstallarla.
2. Nel file IdentityModels.cs nella cartella Models eliminare o impostare come commento le classi **ApplicationUser** e **ApplicationDbContext** . In un'applicazione MVC è possibile eliminare l'intero file IdentityModels.cs. In un'applicazione Web Form, eliminare le due classi, ma assicurarsi di lasciare la classe helper che si trova anche nel file IdentityModels.cs.
3. Se il provider di archiviazione si trova in un progetto separato, aggiungervi un riferimento nell'applicazione Web.
4. Sostituire tutti i riferimenti a `using Microsoft.AspNet.Identity.EntityFramework;` con un'istruzione using per lo spazio dei nomi del provider di archiviazione.
5. Nella classe **Startup.auth.cs** modificare il metodo **ConfigureAuth** in modo da usare una singola istanza del contesto appropriato. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Nella cartella di avvio dell'app\_aprire **IdentityConfig.cs**. Nella classe ApplicationUserManager modificare il metodo **create** per restituire un gestore utenti che usa l'archivio utente personalizzato. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Sostituire tutti i riferimenti a **ApplicationUser** con **IdentityUser**.
8. Il progetto predefinito include alcuni membri della classe User che non sono definiti nell'Interfaccia IUser; ad esempio posta elettronica, PasswordHash e GenerateUserIdentityAsync. Se la classe utente non dispone di questi membri, sarà necessario implementarli o modificare il codice che utilizza tali membri.
9. Se sono state create istanze di RoleManager, modificare il codice in modo da usare la nuova classe nell'rolestore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Il progetto predefinito è progettato per una classe utente che dispone di un valore stringa per la chiave. Se la classe utente ha un tipo diverso per la chiave, ad esempio un numero intero, è necessario modificare il progetto in modo che funzioni con il tipo. Vedere [modificare la chiave primaria per gli utenti in ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Se necessario, aggiungere la stringa di connessione al file Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Altre risorse

- Blog: [implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Esercitazione e codice GIT: [Simple. Data ASP.NET Identity Provider](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Esercitazione:[configurazione degli account Identity di base e puntando a un database esterno](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Per [@xivSolutions](https://twitter.com/xivSolutions).
- Esercitazione[: implementazione di un provider di archiviazione ASP.NET Identity MySQL personalizzato](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entità codefluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) per [SoftFluent](http://www.softfluent.com/)
- [Archiviazione tabelle di Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) di James Randall.
- Archiviazione tabelle di Azure: [ASPNET. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) by [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloudy di Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastic eseg[h: identità elastica](https://github.com/bmbsqd/elastic-identity) di Bombsquad ab.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) di Jonathan Sheely Jonathan Sheely.
- [NHibernate. AspNet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) di Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) per [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. AspNet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) di [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis. AspNet. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelli T4 per generare il codice EF per un archivio utente "database First": [ASPNET. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
