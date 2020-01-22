---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementazione di un provider di archiviazione ASP.NET Identity MySQL personalizzato-ASP.NET 4. x
author: raquelsa
description: ASP.NET Identity è un sistema estensibile che consente di creare un provider di archiviazione personalizzato e di inserirlo nell'applicazione senza rielaborare l'applicazione...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519128"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementazione di un provider di archiviazione MySQL personalizzato di ASP.NET Identity

di [Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity è un sistema estensibile che consente di creare un provider di archiviazione personalizzato e di inserirlo nell'applicazione senza riutilizzare l'applicazione. Questo argomento descrive come creare un provider di archiviazione MySQL per ASP.NET Identity. Per una panoramica sulla creazione di provider di archiviazione personalizzati, vedere [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Per completare questa esercitazione, è necessario avere Visual Studio 2013 con Update 2.
> 
> Questa esercitazione:
> 
> - Mostra come creare un'istanza di database MySQL in Azure.
> - Mostra come usare uno strumento client MySQL (MySQL Workbench) per creare tabelle e gestire il database remoto in Azure.
> - Mostra come sostituire l'implementazione predefinita di archiviazione ASP.NET Identity con l'implementazione personalizzata in un progetto di applicazione MVC.
> 
> Questa esercitazione è stata scritta originariamente da Raquel Soares de Almeida e Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ). Il progetto di esempio è stato aggiornato per l'identità 2,0 di Suhas Joshi. L'argomento è stato aggiornato per l'identità 2,0 di Tom FitzMacken.

## <a name="download-completed-project"></a>Scarica progetto completato

Al termine di questa esercitazione, si disporrà di un progetto di applicazione MVC con ASP.NET Identity uso di un database MySQL ospitato in Azure.

È possibile scaricare il provider di archiviazione MySQL completato in [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL).

## <a name="the-steps-you-will-perform"></a>Passaggi da eseguire

In questa esercitazione si apprenderà come:

1. Creare un database MySQL in Azure
2. Creare le tabelle ASP.NET Identity in MySQL
3. Creare un'applicazione MVC e configurarla per l'uso del provider MySQL
4. Eseguire l'app

In questo argomento non viene illustrata l'architettura di ASP.NET Identity e le decisioni da prendere durante l'implementazione di un provider di archiviazione clienti. Per informazioni, vedere [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Esaminare le classi del provider di archiviazione MySQL

Prima di eseguire i passaggi per creare il provider di archiviazione MySQL, esaminiamo le classi che costituiscono il provider di archiviazione. Sono necessarie classi che gestiscono le operazioni e le classi del database chiamate dall'applicazione per gestire utenti e ruoli.

### <a name="storage-classes"></a>Classi di archiviazione

- [IdentityUser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) : contiene le proprietà dell'utente.
- [UserStore ambito](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) : contiene operazioni per l'aggiunta, l'aggiornamento o il recupero degli utenti.
- [IdentityRole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) : contiene le proprietà per i ruoli.
- [Nell'rolestore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contiene operazioni per l'aggiunta, l'eliminazione, l'aggiornamento e il recupero di ruoli.

### <a name="data-access-layer-classes"></a>Classi del livello di accesso ai dati

Per questo esempio, le classi del livello di accesso ai dati contengono istruzioni SQL per l'utilizzo delle tabelle. Tuttavia, nel codice può essere necessario usare il mapping relazionale a oggetti (ORM), ad esempio Entity Framework o NHibernate. In particolare, l'applicazione può comportare prestazioni insufficienti senza un ORM che includa il caricamento lazy e la memorizzazione nella cache degli oggetti. Per ulteriori informazioni, vedere [ASP.NET Identity 2,0 senza Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) : contiene la connessione al database MySQL e i metodi per l'esecuzione di operazioni di database. È stata creata un'istanza di userStore ambito e nell'rolestore con un'istanza di questa classe.
- [RoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contiene le operazioni di database per la tabella in cui sono archiviati i ruoli.
- [UserClaimsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contiene le operazioni di database per la tabella in cui sono archiviate le attestazioni utente.
- [UserLoginsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contiene le operazioni di database per la tabella in cui sono archiviate le informazioni di accesso dell'utente.
- [UserRoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) : contiene le operazioni di database per la tabella in cui sono archiviati gli utenti a cui sono assegnati i ruoli.
- [UserTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) : contiene le operazioni di database per la tabella in cui sono archiviati gli utenti.

## <a name="create-a-mysql-database-instance-on-azure"></a>Creare un'istanza di database MySQL in Azure

1. Accedere al [portale di Azure](https://manage.windowsazure.com/).
2. Fare clic su **+ nuovo** nella parte inferiore della pagina e quindi selezionare **Archivia**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Nella procedura guidata **Scegli e componente** aggiuntivo selezionare **database MySQL ClearDB** e fare clic sulla freccia avanti nella parte inferiore destra della finestra di dialogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantieni il piano **gratuito** predefinito e modifica il **nome** in **IdentityMySQLDatabase**. Selezionare l'area più vicina, quindi fare clic sulla freccia avanti.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Fare clic sul segno di spunta per completare la creazione del database.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Dopo che è stato creato, il database può essere gestito nella scheda **ADD-ONS** del portale di gestione.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. È possibile ottenere le informazioni di connessione al database facendo clic su **informazioni di connessione** nella parte inferiore della pagina.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copiare la stringa di connessione facendo clic sul pulsante Copy (copia) e salvarla in modo che sia possibile usarla in un secondo momento nell'applicazione MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Creare le tabelle ASP.NET Identity in un database MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installare lo strumento MySQL Workbench per connettersi e gestire il database MySQL

1. Installare lo strumento **MySQL Workbench** dalla [pagina dei download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Avviare l'app e aggiungere clic sul pulsante **MySQLConnections +** per aggiungere una nuova connessione. Usare i dati della stringa di connessione copiati dal database MySQL di Azure creato in precedenza in questa esercitazione.
3. Dopo aver stabilito la connessione, aprire una nuova scheda della **query** ; incollare i comandi da [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) nella query ed eseguirli per creare le tabelle di database.
4. Sono ora disponibili tutte le tabelle ASP.NET Identity necessarie create in un database MySQL ospitato in Azure, come illustrato di seguito.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Creare un progetto di applicazione MVC da un modello e configurarlo per l'uso del provider MySQL

Se necessario, installare [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con l'aggiornamento 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Scaricare il progetto ASP. NET. Identity. MySQL da GitHub

1. Passare all'URL del repository in [ASPNET. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Scaricare il codice sorgente.
3. Estrarre il file zip in una cartella locale.
4. Aprire la soluzione AspNet. Identity. MySQL e compilarla.

### <a name="create-a-new-mvc-application-project-from-template"></a>Creare un nuovo progetto di applicazione MVC da un modello

1. Fare clic con il pulsante destro del mouse sulla soluzione **ASPNET. Identity. MySQL** e scegliere **Aggiungi**, **nuovo progetto**
2. Nella finestra di dialogo **Aggiungi nuovo progetto** selezionare oggetto  **C# visivo** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Assegnare al progetto il nome **IdentityMySQLDemo**; e quindi fare clic su OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello MVC con le opzioni predefinite (che includono **singoli account utente** come metodo di autenticazione) e fare clic su **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto IdentityMySQLDemo e scegliere **Gestisci pacchetti NuGet**. Nella finestra di dialogo Cerca casella di testo digitare **Identity. EntityFramework**. Selezionare il pacchetto nell'elenco dei risultati e fare clic su **Disinstalla**. Verrà richiesto di disinstallare il pacchetto di dipendenze EntityFramework. Fare clic su Sì perché il pacchetto non sarà più disponibile nell'applicazione.
5. Fare clic con il pulsante destro del mouse sul progetto IdentityMySQLDemo, scegliere **Aggiungi**, **riferimento, soluzione, progetti** , selezionare il progetto ASPNET. Identity. MySQL e fare clic su **OK**.
6. Nel progetto IdentityMySQLDemo sostituire tutti i riferimenti a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   Con  
     `using AspNet.Identity.MySQL;`
7. In IdentityModels.cs impostare **ApplicationDbContext** per derivare da **MySqlDatabase** e includere un costruttore che accetta un solo parametro con il nome della connessione.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Aprire il file IdentityConfig.cs. Nel metodo **ApplicationUserManager. Create** sostituire l'istanza di usermanager con il codice seguente:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Aprire il file Web. config e sostituire la stringa DefaultConnection con questa voce sostituendo i valori evidenziati con la stringa di connessione del database MySQL creato nei passaggi precedenti:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Eseguire l'app e connettersi al database MySQL

1. Fare clic con il pulsante destro del mouse sul progetto **IdentityMySQLDemo** e selezionare **Imposta come progetto di avvio**
2. Premere **CTRL + F5** per compilare ed eseguire l'app.
3. Fare clic sulla scheda **Register (registra** ) nella parte superiore della pagina.
4. Immettere un nuovo nome utente e una nuova password, quindi fare clic su **registra**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Il nuovo utente è ora registrato e connesso.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Tornare allo strumento MySQL Workbench ed esaminare il contenuto della tabella **IdentityMySQLDatabase** . Controllare la tabella utenti per le voci durante la registrazione di nuovi utenti.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come abilitare altri metodi di autenticazione in questa app, vedere [creare un'app ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Per informazioni su come integrare il database con OAuth e per configurare i ruoli per limitare l'accesso degli utenti all'app, vedere [distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
