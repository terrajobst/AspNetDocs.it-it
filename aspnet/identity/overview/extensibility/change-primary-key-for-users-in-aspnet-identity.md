---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modificare la chiave primaria per gli utenti in ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: In Visual Studio 2013, l'applicazione Web predefinita utilizza un valore stringa per la chiave per gli account utente. ASP.NET Identity consente di modificare il tipo di...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519141"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Modifica della chiave primaria per gli utenti in ASP.NET Identity

da [Tom FitzMacken](https://github.com/tfitzmac)

> In Visual Studio 2013, l'applicazione Web predefinita utilizza un valore stringa per la chiave per gli account utente. ASP.NET Identity consente di modificare il tipo di chiave per soddisfare i requisiti dei dati. Ad esempio, è possibile modificare il tipo della chiave da una stringa a un numero intero.
> 
> In questo argomento viene illustrato come iniziare con l'applicazione Web predefinita e come impostare la chiave dell'account utente su un numero intero. È possibile utilizzare le stesse modifiche per implementare qualsiasi tipo di chiave nel progetto. Mostra come apportare queste modifiche nell'applicazione Web predefinita, ma è possibile applicare modifiche simili a un'applicazione personalizzata. Mostra le modifiche necessarie quando si lavora con MVC o Web Form.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Visual Studio 2013 con Update 2 (o versione successiva)
> - ASP.NET Identity 2,1 o versione successiva

Per eseguire la procedura descritta in questa esercitazione, è necessario avere Visual Studio 2013 Update 2 (o versione successiva) e un'applicazione Web creata dal modello di applicazione Web ASP.NET. Il modello è stato modificato nell'aggiornamento 3. In questo argomento viene illustrato come modificare il modello in Update 2 e Update 3.

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Modificare il tipo della chiave nella classe utente Identity](#userclass)
- [Aggiungere classi Identity personalizzate che usano il tipo di chiave](#customclass)
- [Modificare la classe del contesto e il gestore utenti per usare il tipo di chiave](#context)
- [Modificare la configurazione di avvio per usare il tipo di chiave](#startup)
- [Per MVC con Update 2, modificare AccountController per passare il tipo di chiave](#mvcupdate2)
- [Per MVC con Update 3, modificare AccountController e ManageController per passare il tipo di chiave](#mvcupdate3)
- [Per i Web Form con Update 2, modificare le pagine dell'account per passare il tipo di chiave](#webformsupdate2)
- [Per i Web Form con Update 3, modificare le pagine dell'account per passare il tipo di chiave](#webformsupdate3)
- [Esegui applicazione](#run)
- [Altre risorse](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Modificare il tipo della chiave nella classe utente Identity

Nel progetto creato dal modello di applicazione Web ASP.NET, specificare che la classe ApplicationUser usa un numero intero per la chiave per gli account utente. In IdentityModels.cs modificare la classe ApplicationUser in modo da ereditare da IdentityUser con tipo **int** per il parametro generico TKey. È anche possibile passare i nomi di tre classi personalizzate che non sono state ancora implementate.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Il tipo della chiave è stato modificato, ma, per impostazione predefinita, il resto dell'applicazione presuppone ancora che la chiave sia una stringa. È necessario indicare in modo esplicito il tipo di chiave nel codice che presuppone una stringa.

Nella classe **ApplicationUser** modificare il metodo **GenerateUserIdentityAsync** in modo da includere int, come illustrato nel codice evidenziato di seguito. Questa modifica non è necessaria per i progetti Web Form con il modello Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Aggiungere classi Identity personalizzate che usano il tipo di chiave

Le altre classi Identity, ad esempio IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, userStore ambito, nell'rolestore, sono ancora configurate per l'uso di una chiave di stringa. Creare nuove versioni di queste classi che specificano un Integer per la chiave. In queste classi non è necessario fornire un codice di implementazione molto elevato, ma è sufficiente impostare int come chiave.

Aggiungere le classi seguenti al file IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Modificare la classe del contesto e il gestore utenti per usare il tipo di chiave

In IdentityModels.cs modificare la definizione della classe **ApplicationDbContext** in modo da usare le nuove classi personalizzate e un valore **int** per la chiave, come illustrato nel codice evidenziato.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Il parametro ThrowIfV1Schema non è più valido nel costruttore. Modificare il costruttore in modo che non passi un valore ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Aprire IdentityConfig.cs e modificare la classe **ApplicationUserManger** in modo da usare la nuova classe archivio utenti per salvare in modo permanente i dati e un **int** per la chiave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Nel modello Update 3 è necessario modificare la classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Modificare la configurazione di avvio per usare il tipo di chiave

In Startup.Auth.cs sostituire il codice OnValidateIdentity, come illustrato di seguito. Si noti che la definizione getUserIdCallback analizza il valore stringa in un numero intero.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Se il progetto non riconosce l'implementazione generica del metodo **GetUserID** , potrebbe essere necessario aggiornare il pacchetto NuGet ASP.NET Identity alla versione 2,1

Sono state apportate numerose modifiche alle classi di infrastruttura usate da ASP.NET Identity. Se si tenta di compilare il progetto, si noterà un numero elevato di errori. Fortunatamente, gli errori rimanenti sono tutti simili. La classe Identity prevede un numero intero per la chiave, ma il controller (o Web Form) passa un valore stringa. In ogni caso, è necessario eseguire la conversione da una stringa a un valore integer chiamando **GetUserID&lt;int&gt;** . È possibile utilizzare l'elenco errori dalla compilazione o seguire le modifiche riportate di seguito.

Le modifiche rimanenti dipendono dal tipo di progetto che si sta creando e dall'aggiornamento installato in Visual Studio. È possibile passare direttamente alla sezione pertinente tramite i collegamenti seguenti

- [Per MVC con Update 2, modificare AccountController per passare il tipo di chiave](#mvcupdate2)
- [Per MVC con Update 3, modificare AccountController e ManageController per passare il tipo di chiave](#mvcupdate3)
- [Per i Web Form con Update 2, modificare le pagine dell'account per passare il tipo di chiave](#webformsupdate2)
- [Per i Web Form con Update 3, modificare le pagine dell'account per passare il tipo di chiave](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Per MVC con Update 2, modificare AccountController per passare il tipo di chiave

Aprire il file AccountController.cs. È necessario modificare i metodi seguenti.

Metodo **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Dissocia** metodo

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage (ManageUserViewModel) (metodo)**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

Metodo **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

Metodo **RemoveAccountList**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

Metodo **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Per MVC con Update 3, modificare AccountController e ManageController per passare il tipo di chiave

Aprire il file AccountController.cs. È necessario modificare il metodo seguente.

Metodo **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

Metodo **SendCode**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Aprire il file ManageController.cs. È necessario modificare i metodi seguenti.

**Index** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

Metodi **RemoveLogin**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

Metodo **AddPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

Metodo **EnableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

Metodo **DisableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

Metodi **VerifyPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

Metodo **RemovePhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

Metodo **Sepassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

Metodo **ManageLogins**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

Metodo **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

Metodo **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

Metodo **HasPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Per i Web Form con Update 2, modificare le pagine dell'account per passare il tipo di chiave

Per i Web Form con Update 2, è necessario modificare le pagine seguenti.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Per i Web Form con Update 3, modificare le pagine dell'account per passare il tipo di chiave

Per i Web Form con Update 3, è necessario modificare le pagine seguenti.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Eseguire l'applicazione

Sono state completate tutte le modifiche necessarie al modello di applicazione Web predefinito. Eseguire l'applicazione e registrare un nuovo utente. Dopo la registrazione dell'utente, si noterà che la tabella AspNetUsers contiene una colonna ID che corrisponde a un numero intero.

![nuova chiave primaria](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Se in precedenza sono state create le tabelle ASP.NET Identity con una chiave primaria diversa, è necessario apportare alcune modifiche aggiuntive. Se possibile, è sufficiente eliminare il database esistente. Il database verrà ricreato con la progettazione corretta quando si esegue l'applicazione Web e si aggiunge un nuovo utente. Se l'eliminazione non è possibile, eseguire migrazioni Code First per modificare le tabelle. Tuttavia, la nuova chiave primaria integer non verrà configurata come proprietà di identità SQL nel database. È necessario impostare manualmente la colonna ID come identità.

<a id="other"></a>
## <a name="other-resources"></a>Altre risorse

- [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrazione dei dati del provider universale per l'appartenenza e i profili utente a ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Applicazione di esempio](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) con chiave primaria modificata
