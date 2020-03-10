---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Aggiunta di ASP.NET Identity a un progetto Web Form vuoto o esistente-ASP.NET 4. x
author: raquelsa
description: Questa esercitazione illustra come aggiungere ASP.NET Identity (il sistema di appartenenze per ASP.NET) a un'applicazione ASP.NET. Quando si crea un nuovo Web Form o MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584126"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Aggiunta di ASP.NET Identity a un progetto Web Forms vuoto o esistente

> Questa esercitazione illustra come aggiungere [ASP.NET Identity](introduction-to-aspnet-identity.md) (il nuovo sistema di appartenenza per ASP.NET) a un'applicazione ASP.NET.
> 
> Quando si crea un nuovo progetto Web Form o MVC in Visual Studio 2017 RTM con singoli account, Visual Studio installerà tutti i pacchetti necessari e aggiungerà tutte le classi necessarie. In questa esercitazione vengono illustrati i passaggi per aggiungere ASP.NET Identity supporto al progetto Web Form esistente o a un nuovo progetto vuoto. Illustrerò tutti i pacchetti NuGet che è necessario installare e le classi da aggiungere. Verranno esaminati i Web Form di esempio per la registrazione di nuovi utenti e l'accesso, evidenziando tutte le API principali del punto di ingresso per la gestione e l'autenticazione degli utenti. In questo esempio verrà utilizzata l'implementazione predefinita di ASP.NET Identity per l'archiviazione di dati SQL basata su Entity Framework. In questa esercitazione verrà usato il database locale per il database SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Inizia a usare ASP.NET Identity

1. Per iniziare, installare ed eseguire [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Selezionare **nuovo progetto** dalla pagina iniziale oppure è possibile usare il menu e selezionare **file**, quindi **nuovo progetto**.
3. Nel riquadro sinistro espandere oggetto **visivo C#** , quindi selezionare **Web**, quindi **applicazione Web ASP.NET (.NET Framework)** . Assegnare al progetto il nome "WebFormsIdentity" e fare clic su **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** .
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Si noti che il pulsante **Cambia autenticazione** è disabilitato e non è disponibile alcun supporto per l'autenticazione in questo modello. I modelli Web Form, MVC e API Web consentono di selezionare l'approccio di autenticazione.

## <a name="add-identity-packages-to-your-app"></a>Aggiungere pacchetti di identità all'app

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**. Cercare e installare il pacchetto **Microsoft. AspNet. Identity. EntityFramework** . 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Si noti che questo pacchetto installerà i pacchetti di dipendenza: **EntityFramework** e **Microsoft ASP.NET Identity Core**.

## <a name="add-a-web-form-to-register-users"></a>Aggiungere un Web Form per registrare gli utenti

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi **Web Form**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Nella finestra di dialogo **Specifica nome per l'elemento** assegnare un nome al nuovo **Registro**Web Form, quindi selezionare **OK** .
3. Sostituire il markup nel file *Register. aspx* generato con il codice riportato di seguito. Le modifiche al codice sono evidenziate. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Si tratta semplicemente di una versione semplificata del file *Register. aspx* creato quando si crea un nuovo progetto Web Form ASP.NET. Il markup precedente aggiunge campi del modulo e un pulsante per registrare un nuovo utente.
4. Aprire il file *Register.aspx.cs* e sostituire il contenuto del file con il codice seguente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Il codice precedente è una versione semplificata del file *Register.aspx.cs* creato quando si crea un nuovo progetto Web Form ASP.NET.
    > 2. La classe *IdentityUser* è l'implementazione predefinita di EntityFramework dell'interfaccia *IUser* . L'interfaccia *IUser* è l'interfaccia minima per un utente in ASP.NET Identity core.
    > 3. La classe *userStore ambito* è l'implementazione EntityFramework predefinita di un archivio utente. Questa classe implementa le interfacce minime del ASP.NET Identity core, ovvero *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore*.
    > 4. La classe *usermanager* espone le API correlate all'utente che salveranno automaticamente le modifiche apportate a *userStore ambito*.
    > 5. La classe *IdentityResult* rappresenta il risultato di un'operazione di identità.
5. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, **Aggiungi cartella ASP.NET** e quindi **dati\_app**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Aprire il file *Web. config* e aggiungere una voce della stringa di connessione per il database che si utilizzerà per archiviare le informazioni utente. Il database verrà creato in fase di esecuzione da EntityFramework per le entità Identity. La stringa di connessione è simile a una creata automaticamente quando si crea un nuovo progetto Web Form. Il codice evidenziato Mostra il markup che è necessario aggiungere:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Per Visual Studio 2015 o versione successiva, sostituire `(localdb)\v11.0` con `(localdb)\MSSQLLocalDB` nella stringa di connessione.
    
7. Fare clic con il pulsante destro del mouse su file *Register. aspx* nel progetto e selezionare **Imposta come pagina iniziale**. Premere CTRL + F5 per compilare ed eseguire l'applicazione Web. Immettere un nuovo nome utente e una nuova password, quindi selezionare **registra**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity dispone del supporto per la convalida e in questo esempio è possibile verificare il comportamento predefinito sui validator utente e password che provengono dal pacchetto Identity core. Il validator predefinito per User (`UserValidator`) dispone di una proprietà `AllowOnlyAlphanumericUserNames` il cui valore predefinito è impostato su `true`. Il validator predefinito per la password (`MinimumLengthValidator`) garantisce che la password contenga almeno 6 caratteri. Questi validator sono proprietà di `UserManager` di cui è possibile eseguire l'override se si desidera eseguire la convalida personalizzata,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verificare il database di identità e le tabelle del database locale generati da Entity Framework

1. Scegliere **Esplora server**dal menu **Visualizza** .
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Espandere **DefaultConnection (WebFormsIdentity)** , espandere **tabelle**, fare clic con il pulsante destro del mouse su **AspNetUsers** , quindi scegliere **Mostra dati tabella**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configurare l'applicazione per l'autenticazione OWIN

A questo punto è stato aggiunto il supporto per la creazione di utenti. Ora verrà illustrato come è possibile aggiungere l'autenticazione per l'accesso a un utente. ASP.NET Identity usa il middleware di autenticazione di Microsoft OWIN per l'autenticazione basata su form. L'autenticazione del cookie OWIN è un meccanismo di autenticazione basato su cookie e attestazioni che può essere usato da qualsiasi framework ospitato in [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) o IIS. Con questo modello, è possibile usare gli stessi pacchetti di autenticazione in più Framework, tra cui ASP.NET MVC e Web Form. Per altre informazioni sul progetto Katana e su come eseguire il middleware in un host agnostico, vedere [Introduzione con il progetto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Installare i pacchetti di autenticazione nell'applicazione

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**. Cercare e installare il pacchetto ***Microsoft. AspNet. Identity. Owin*** . 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Cercare e installare il pacchetto ***Microsoft. Owin. host. systemWeb*** .

    > [!NOTE]
    > Il pacchetto **Microsoft. AspNet. Identity. Owin** contiene un set di classi di estensione Owin per gestire e configurare il middleware di autenticazione Owin che deve essere utilizzato dai pacchetti ASP.NET Identity core.
    > Il pacchetto **Microsoft. Owin. host. systemWeb** contiene un server Owin che consente l'esecuzione di applicazioni basate su OWIN in IIS tramite la pipeline delle richieste ASP.NET. Per altre informazioni, vedere [il middleware OWIN nella pipeline integrata di IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Aggiungere classi di configurazione di avvio e autenticazione OWIN

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi**, quindi **Aggiungi nuovo elemento**. Nella finestra di dialogo Cerca casella di testo digitare "*Owin*". Assegnare alla classe il nome "*Startup*" e selezionare **Aggiungi**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Nel file Startup.cs aggiungere il codice evidenziato riportato di seguito per configurare l'autenticazione di OWIN cookie.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Questa classe contiene l'attributo `OwinStartup` per specificare la classe di avvio OWIN. Ogni applicazione OWIN dispone di una classe startup in cui è possibile specificare i componenti per la pipeline dell'applicazione. Per altre informazioni su questo modello, vedere [rilevamento della classe di avvio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) .

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Aggiungere Web Form per la registrazione e l'accesso degli utenti

1. Aprire il file *Register.aspx.cs* e aggiungere il codice seguente che esegue l'accesso all'utente quando la registrazione ha esito positivo.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Poiché l'autenticazione dei cookie ASP.NET Identity e OWIN è basata su attestazioni, il Framework richiede che lo sviluppatore di app generi un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) per l'utente. ClaimsIdentity contiene informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente. In questa fase è anche possibile aggiungere altre attestazioni per l'utente.
    > - È possibile accedere all'utente usando AuthenticationManager da OWIN e chiamando `SignIn` e passando il ClaimsIdentity come illustrato in precedenza. Questo codice effettuerà l'accesso dell'utente e genererà anche un cookie. Questa chiamata è analoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usato dal modulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi**, quindi **Web Form**. Assegnare un nome all' **account di accesso**Web Form.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Sostituire il contenuto del file *login. aspx* con il codice seguente:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Sostituire il contenuto del file *login.aspx.cs* con il codice seguente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Il `Page_Load` ora verifica lo stato dell'utente corrente ed esegue un'azione in base al relativo stato di `Context.User.Identity.IsAuthenticated`.
    >   **Visualizzazione del nome utente connesso** : il Framework di identità Microsoft ASP.NET ha aggiunto metodi di estensione su [System. Security. Principal. IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) che consente di ottenere il `UserName` e `UserId` per l'utente connesso. Questi metodi di estensione sono definiti nell'assembly `Microsoft.AspNet.Identity.Core`. Questi metodi di estensione sono la sostituzione di [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Metodo SignIn: `This` metodo sostituisce il metodo `CreateUser_Click` precedente in questo esempio e ora accede all'utente dopo la creazione dell'utente.   
    >   Microsoft OWIN Framework ha aggiunto metodi di estensione su `System.Web.HttpContext` che consente di ottenere un riferimento a un `IOwinContext`. Questi metodi di estensione sono definiti nell'assembly `Microsoft.Owin.Host.SystemWeb`. La classe `OwinContext` espone una proprietà `IAuthenticationManager` che rappresenta la funzionalità del middleware di autenticazione disponibile nella richiesta corrente. È possibile accedere all'utente usando il `AuthenticationManager` da OWIN e chiamando `SignIn` e passando il `ClaimsIdentity` come illustrato in precedenza. Poiché l'autenticazione dei cookie ASP.NET Identity e OWIN è un sistema basato su attestazioni, il Framework richiede che l'app generi un `ClaimsIdentity` per l'utente. Il `ClaimsIdentity` dispone di informazioni su tutte le attestazioni per l'utente, ad esempio i ruoli a cui appartiene l'utente. In questa fase è anche possibile aggiungere altre attestazioni per l'utente. questo codice effettuerà l'accesso dell'utente e genererà anche un cookie. Questa chiamata è analoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usato dal modulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
    > - Metodo `SignOut`: ottiene un riferimento al `AuthenticationManager` da OWIN e chiama `SignOut`. Questo è analogo al metodo [FormsAuthentication. Sign](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) out usato dal modulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
5. Premere **CTRL + F5** per compilare ed eseguire l'applicazione Web. Immettere un nuovo nome utente e una nuova password, quindi selezionare **registra**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Nota: a questo punto, il nuovo utente viene creato e connesso.
6. Selezionare il pulsante **Disconnetti** . Si verrà reindirizzati al modulo di accesso.
7. Immettere un nome utente o una password non valida e selezionare il pulsante **Accedi** . 
   Il metodo `UserManager.Find` restituirà null e verrà visualizzato il messaggio di errore: " *nome utente o password non valida* ".
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
