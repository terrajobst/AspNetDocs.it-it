---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrazione dei dati del provider universale per l'appartenenza e i profiliC#utente a ASP.NET Identity ()-ASP.NET 4. x
author: rustd
description: Questa esercitazione descrive i passaggi necessari per eseguire la migrazione dei dati utente e del ruolo e dei dati del profilo utente creati usando provider universali di un'app esistente...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456114"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrazione dei dati dei provider universali per l'appartenenza e i profili utente ad ASP.NET Identity (C#)

di Manuel [Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> In questa esercitazione vengono descritti i passaggi necessari per eseguire la migrazione di dati utente e ruolo e dei dati del profilo utente creati utilizzando provider universali di un'applicazione esistente al modello di ASP.NET Identity. L'approccio indicato qui per la migrazione dei dati del profilo utente può essere usato anche in un'applicazione con l'appartenenza a SQL.

Con [il rilascio di](../../index.md)Visual Studio 2013, il team di ASP.NET ha introdotto un nuovo sistema di ASP.NET Identity ed è possibile leggere altre informazioni sulla versione. In questo articolo vengono illustrati i passaggi per eseguire la migrazione delle applicazioni Web dall' [appartenenza SQL al nuovo sistema di identità](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), in questo articolo vengono illustrati i passaggi per eseguire la migrazione di applicazioni esistenti che seguono il modello di provider per la gestione di utenti e ruoli al nuovo modello di identità. Questa esercitazione si concentra principalmente sulla migrazione dei dati del profilo utente per collegarli facilmente al nuovo sistema. La migrazione delle informazioni relative a utenti e ruoli è simile per l'appartenenza a SQL. L'approccio seguito dalla migrazione dei dati di profilo può essere usato anche in un'applicazione con l'appartenenza a SQL.

Come esempio, si inizia con un'app Web creata con Visual Studio 2012 che usa il modello di provider. Si aggiungerà quindi il codice per la gestione dei profili, si registrerà un utente, si aggiungeranno dati di profilo per gli utenti, si eseguirà la migrazione dello schema del database e quindi si modificherà l'applicazione per usare il sistema di identità per la gestione degli utenti e Come test della migrazione, gli utenti creati con provider universali devono essere in grado di accedere e i nuovi utenti devono essere in grado di effettuare la registrazione.

> [!NOTE]
> L'esempio completo è reperibile in [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Riepilogo della migrazione dei dati del profilo

Prima di iniziare con le migrazioni, è ora esaminata l'esperienza di archiviazione dei dati di profilo nel modello di provider. I dati di profilo per gli utenti dell'applicazione possono essere archiviati in diversi modi, più comuni tra loro sono l'uso dei provider di profili incorporati forniti insieme al provider universali. I passaggi includono

1. Aggiungere una classe con le proprietà utilizzate per archiviare i dati del profilo.
2. Aggiungere una classe che estende ' ProfileBase ' e implementa i metodi per ottenere i dati del profilo sopra indicati per l'utente.
3. Abilitare l'utilizzo dei provider di profili predefiniti nel file *Web. config* e definire la classe dichiarata nel passaggio #2 da utilizzare per l'accesso alle informazioni sul profilo.

Le informazioni sul profilo vengono archiviate come dati XML e binari serializzati nella tabella ' profiles ' del database.

Dopo la migrazione dell'applicazione per l'utilizzo del nuovo sistema di ASP.NET Identity, le informazioni sul profilo vengono deserializzate e archiviate come proprietà nella classe utente. Ogni proprietà può quindi essere mappata sulle colonne della tabella utente. Il vantaggio è che le proprietà possono essere usate direttamente usando la classe utente, oltre a non dover serializzare/deserializzare le informazioni sui dati ogni volta che si accede a tale classe.

## <a name="getting-started"></a>Guida introduttiva

1. Creare una nuova applicazione Web Form ASP.NET 4,5 in Visual Studio 2012. Nell'esempio corrente viene utilizzato il modello Web Form, ma è possibile utilizzare anche l'applicazione MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Creare una nuova cartella ' Models ' per archiviare le informazioni sul profilo  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Ad esempio, è possibile archiviare la data di nascita, città, altezza e peso dell'utente nel profilo. L'altezza e il peso vengono archiviati come una classe personalizzata denominata ' PersonalStats '. Per archiviare e recuperare il profilo, è necessaria una classe che estende ' ProfileBase '. Viene ora creata una nuova classe ' AppProfile ' per ottenere e archiviare le informazioni sul profilo.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Abilitare il profilo nel file *Web. config* . Immettere il nome della classe da usare per archiviare/recuperare le informazioni utente create nel passaggio #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Aggiungere una pagina Web Form nella cartella "account" per ottenere i dati di profilo dall'utente e archiviarli. Fare clic con il pulsante destro del mouse sul progetto e scegliere ' Aggiungi nuovo elemento '. Aggiungere una nuova pagina Web Form con la pagina master "AddProfileData. aspx". Copiare quanto segue nella sezione ' MainContent ':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Aggiungere il codice seguente nel code-behind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Aggiungere lo spazio dei nomi in cui è definita la classe AppProfile per rimuovere gli errori di compilazione.
6. Eseguire l'app e creare un nuovo utente con nome utente '**Olduser '.** Passare alla pagina "AddProfileData" e aggiungere le informazioni sul profilo per l'utente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

È possibile verificare che i dati vengano archiviati come XML serializzato nella tabella ' profiles ' usando la finestra Esplora server. In Visual Studio scegliere "Esplora server" dal menu "Visualizza". Deve essere presente una connessione dati per il database definito nel file *Web. config* . Facendo clic sulla connessione dati vengono visualizzate diverse sottocategorie. Espandere ' tabelle ' per visualizzare le diverse tabelle del database, quindi fare clic con il pulsante destro del mouse su' profili ' e scegliere ' Mostra dati tabella ' per visualizzare i dati del profilo archiviati nella tabella profili.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrazione dello schema del database

Per far funzionare il database esistente con il sistema di identità, è necessario aggiornare lo schema nel database di identità per supportare i campi aggiunti al database originale. Questa operazione può essere eseguita usando gli script SQL per creare nuove tabelle e copiare le informazioni esistenti. Nella finestra ' Esplora server ' espandere ' DefaultConnection ' per visualizzare le tabelle. Fare clic con il pulsante destro del mouse su tabelle e scegliere ' nuova query '

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Incollare lo script SQL da [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ed eseguirlo. Se ' DefaultConnection ' viene aggiornato, si noterà che vengono aggiunte le nuove tabelle. È possibile controllare i dati all'interno delle tabelle per vedere che è stata eseguita la migrazione delle informazioni.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrazione dell'applicazione per l'utilizzo di ASP.NET Identity

1. Installare i pacchetti NuGet necessari per ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Altre informazioni sulla gestione dei pacchetti NuGet sono disponibili [qui](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Per lavorare con i dati esistenti nella tabella, è necessario creare classi di modello che eseguono il mapping alle tabelle e collegarle nel sistema di identità. Come parte del contratto di identità, le classi del modello devono implementare le interfacce definite nella dll Identity. core oppure possono estendere l'implementazione esistente di queste interfacce disponibili in Microsoft. AspNet. Identity. EntityFramework. Verranno utilizzate le classi esistenti per ruolo, account di accesso utente e attestazioni utente. Per l'esempio è necessario usare un utente personalizzato. Fare clic con il pulsante destro del mouse sul progetto e creare la nuova cartella ' IdentityModels '. Aggiungere una nuova classe "User" come illustrato di seguito:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Si noti che "ProfileInfo" è ora una proprietà nella classe User. Di conseguenza, è possibile usare la classe utente per lavorare direttamente con i dati del profilo.

Copiare i file nelle cartelle **IdentityModels** e **IdentityAccount** dall'origine download ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Queste sono le classi del modello rimanenti e le nuove pagine necessarie per la gestione di utenti e ruoli usando le API ASP.NET Identity. L'approccio usato è simile all'appartenenza a SQL e la spiegazione dettagliata è disponibile [qui](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copia dei dati del profilo nelle nuove tabelle

Come indicato in precedenza, è necessario deserializzare i dati XML nelle tabelle dei profili e archiviarli nelle colonne della tabella AspNetUsers. Le nuove colonne sono state create nella tabella Users nel passaggio precedente, in modo che tutte le colonne rimangano popolate con i dati necessari. A tale scopo, si userà un'applicazione console che viene eseguita una volta per popolare le colonne appena create nella tabella users.

1. Creare una nuova applicazione console nella soluzione in uscita.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installare la versione più recente del pacchetto di Entity Framework.
3. Aggiungere l'applicazione Web creata in precedenza come riferimento all'applicazione console. A tale scopo, fare clic con il pulsante destro del mouse su Project, quindi scegliere "Add References", quindi Solution, quindi fare clic sul progetto e fare clic su OK.
4. Copiare il codice seguente nella classe Program.cs. Questa logica legge i dati di profilo per ogni utente, li serializza come oggetto ' ProfileInfo ' e li archivia nuovamente nel database.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Alcuni modelli utilizzati sono definiti nella cartella ' IdentityModels ' del progetto di applicazione Web, pertanto è necessario includere gli spazi dei nomi corrispondenti.
5. Il codice precedente funziona nel file di database nella cartella app\_data del progetto di applicazione Web creato nei passaggi precedenti. Per fare riferimento a tale stringa, aggiornare la stringa di connessione nel file app. config dell'applicazione console con la stringa di connessione nel file Web. config dell'applicazione Web. Specificare anche il percorso fisico completo nella proprietà' AttachDbFilename '.
6. Aprire un prompt dei comandi e passare alla cartella bin dell'applicazione console precedente. Eseguire il file eseguibile ed esaminare l'output del log, come illustrato nella figura seguente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Aprire la tabella ' AspNetUsers ' nel Esplora server e verificare i dati nelle nuove colonne che contengono le proprietà. Devono essere aggiornati con i valori di proprietà corrispondenti.

## <a name="verify-functionality"></a>Verificare la funzionalità

Utilizzare le pagine di appartenenza appena aggiunte implementate utilizzando ASP.NET Identity per accedere a un utente dal database precedente. L'utente deve essere in grado di accedere usando le stesse credenziali. Provare le altre funzionalità, ad esempio l'aggiunta di OAuth, la creazione di un nuovo utente, la modifica di una password, l'aggiunta di ruoli, l'aggiunta di utenti ai ruoli e così via.

I dati del profilo per l'utente precedente e i nuovi utenti devono essere recuperati e archiviati nella tabella users. Non è più possibile fare riferimento alla tabella precedente.

## <a name="conclusion"></a>Conclusione

L'articolo descrive il processo di migrazione di applicazioni Web che utilizzano il modello di provider per l'appartenenza a ASP.NET Identity. L'articolo ha inoltre illustrato come eseguire la migrazione dei dati di profilo per consentire agli utenti di essere collegati al sistema di identità. Per domande e problemi riscontrati durante la migrazione dell'app, lasciare commenti di seguito.

*Grazie a Rick Anderson e Robert McMurray per la revisione dell'articolo.*
