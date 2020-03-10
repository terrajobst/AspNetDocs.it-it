---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "ASP.NET Identity: uso dell'archiviazione MySQL con un provider MySQL EntityFrameworkC#()-ASP.NET 4. x"
author: maumar
description: Questa esercitazione illustra come sostituire il meccanismo di archiviazione dei dati predefinito per ASP.NET Identity con EntityFramework (provider client SQL) con un provid MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584007"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: uso dell'archiviazione MySQL con un provider MySQL EntityFrameworkC#()

di [Maurycy Markowski](https://github.com/maumar), [Raquel Soares de Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Questa esercitazione illustra come sostituire il meccanismo di archiviazione dei dati predefinito per [**ASP.NET Identity**](introduction-to-aspnet-identity.md) con EntityFramework (provider client SQL) con un provider MySQL.

In questa esercitazione verranno trattati gli argomenti seguenti:

- Creazione di un database MySQL in Azure
- Creazione di un'applicazione MVC con Visual Studio 2013 modello MVC
- Configurazione di EntityFramework per l'uso con un provider di database MySQL
- Esecuzione dell'applicazione per verificare i risultati

Al termine di questa esercitazione, si disporrà di un'applicazione MVC con l'archivio ASP.NET Identity che usa un database MySQL ospitato in Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Creazione di un'istanza del database MySQL in Azure

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Fare clic su **nuovo** nella parte inferiore della pagina e quindi selezionare **Store**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Nella procedura guidata **Scegli e componente** aggiuntivo selezionare **ClearDB MySQL database**e quindi fare clic sulla freccia **Avanti** nella parte inferiore del frame:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantieni il piano **gratuito** predefinito, modifica il **nome** in **IdentityMySQLDatabase**, seleziona l'area più vicina, quindi fai clic sulla freccia **Avanti** nella parte inferiore del frame:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Fare clic sul segno di spunta **Acquista** per completare la creazione del database.

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Dopo che è stato creato, il database può essere gestito nella scheda **ADD-ONS** del portale di gestione. Per recuperare le informazioni di connessione per il database, fare clic su **informazioni di connessione** nella parte inferiore della pagina:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copiare la stringa di connessione facendo clic sul pulsante Copy (copia) nel campo **CONNECTIONSTRING** e salvarla. Queste informazioni vengono usate più avanti in questa esercitazione per l'applicazione MVC:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Creazione di un progetto di applicazione MVC

Per completare i passaggi descritti in questa sezione dell'esercitazione, è necessario installare [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Una volta installato Visual Studio, attenersi alla procedura seguente per creare un nuovo progetto di applicazione MVC:

1. Aprire Visual Studio 2103.
2. Fare clic su **nuovo progetto** nella pagina **iniziale** . in alternativa, è possibile fare clic sul menu **file** e quindi su **nuovo progetto**:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando viene visualizzata la finestra di dialogo **nuovo progetto** , **espandere C# Visual** nell'elenco dei modelli, quindi fare clic su **Web**e selezionare **applicazione Web ASP.NET**. Assegnare al progetto il nome **IdentityMySQLDemo** e quindi fare clic su **OK**:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare **MVC** templatewith opzioni predefinite. in questo modo, i **singoli account utente** vengono configurati come metodo di autenticazione. Fare clic su **OK**:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurare EntityFramework per l'uso con un database MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aggiornare l'assembly Entity Framework per il progetto

L'applicazione MVC creata dal modello di Visual Studio 2013 contiene un riferimento al pacchetto 6.0.0 di [EntityFramework](http://www.nuget.org/packages/EntityFramework) , ma sono stati apportati aggiornamenti a tale assembly dalla relativa versione che contengono miglioramenti significativi delle prestazioni. Per usare questi ultimi aggiornamenti nell'applicazione, seguire questa procedura.

1. Aprire il progetto MVC in Visual Studio.
2. Fare clic su **strumenti**, quindi fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. La **console di gestione pacchetti** verrà visualizzata nella sezione inferiore di Visual Studio. Digitare &quot;**Update-Package EntityFramework**&quot; e premere INVIO:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installare il provider MySQL per EntityFramework

Per consentire a EntityFramework di connettersi al database MySQL, è necessario installare un provider MySQL. A tale scopo, aprire la **console di gestione pacchetti** e digitare &quot;**Install-Package MySQL. Data. Entity-pre**&quot;, quindi premere INVIO.

> [!NOTE]
> Si tratta di una versione non definitiva dell'assembly e, di conseguenza, può contenere bug. Non usare una versione non definitiva del provider in produzione.

[Fare clic sull'immagine seguente per espanderla.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Apportare modifiche alla configurazione del progetto nel file Web. config per l'applicazione

In questa sezione si configurerà il Entity Framework per usare il provider MySQL appena installato, si registrerà la factory del provider MySQL e si aggiungerà la stringa di connessione da Azure.

> [!NOTE]
> Gli esempi seguenti contengono una versione dell'assembly specifica per MySql. Data. dll. Se la versione dell'assembly viene modificata, sarà necessario modificare le impostazioni di configurazione appropriate con la versione corretta.

1. Aprire il file Web. config del progetto in Visual Studio 2013.
2. Individuare le impostazioni di configurazione seguenti, che definiscono il provider di database predefinito e la factory per il Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Sostituire le impostazioni di configurazione con le seguenti, in modo da configurare il Entity Framework per l'uso del provider MySQL:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Individuare la sezione &lt;connectionStrings&gt; e sostituirla con il codice seguente, che definirà la stringa di connessione per il database MySQL ospitato in Azure (si noti che anche il valore ProviderName è stato modificato rispetto all'originale):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Aggiunta del contesto MigrationHistory personalizzato

Entity Framework Code First usa una tabella **MigrationHistory** per tenere traccia delle modifiche del modello e per garantire la coerenza tra lo schema del database e lo schema concettuale. Tuttavia, questa tabella non funziona per MySQL per impostazione predefinita perché la chiave primaria è troppo grande. Per risolvere questo problema, è necessario ridurre le dimensioni della chiave per tale tabella. A tale scopo, seguire questa procedura:

1. Le informazioni sullo schema per questa tabella vengono acquisite in un **HistoryContext**, che può essere modificato come qualsiasi altro **DbContext**. A tale scopo, aggiungere un nuovo file di classe denominato **MySqlHistoryContext.cs** al progetto e sostituirne il contenuto con il codice seguente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Successivamente, è necessario configurare Entity Framework per l'uso di **HistoryContext**modificati, anziché di uno predefinito. Questa operazione può essere eseguita sfruttando le funzionalità di configurazione basate su codice. A tale scopo, aggiungere il nuovo file di classe denominato **MySqlConfiguration.cs** al progetto e sostituirne il contenuto con:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Creazione di un inizializzatore EntityFramework personalizzato per ApplicationDbContext

Il provider MySQL incluso in questa esercitazione attualmente non supporta le migrazioni di Entity Framework, quindi è necessario usare gli inizializzatori di modello per connettersi al database. Poiché questa esercitazione usa un'istanza di MySQL in Azure, sarà necessario creare un inizializzatore di Entity Framework personalizzato.

> [!NOTE]
> Questo passaggio non è necessario se ci si connette a un'istanza di SQL Server in Azure o se si usa un database ospitato in locale.

Per creare un inizializzatore di Entity Framework personalizzato per MySQL, seguire questa procedura:

1. Aggiungere al progetto un nuovo file di classe denominato **MySqlInitializer.cs** e sostituirne il contenuto con il codice seguente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Aprire il file **IdentityModels.cs** per il progetto, che si trova nella directory **Models** e sostituirne il contenuto con il codice seguente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Esecuzione dell'applicazione e verifica del database

Una volta completati i passaggi nelle sezioni precedenti, è necessario eseguire il test del database. A tale scopo, seguire questa procedura:

1. Premere **CTRL + F5** per compilare ed eseguire l'applicazione Web.
2. Fare clic sulla scheda **Register (registra** ) nella parte superiore della pagina:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Immettere un nuovo nome utente e una nuova password e quindi fare clic su **Register (registra**):

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. A questo punto, le tabelle ASP.NET Identity vengono create nel database MySQL e l'utente viene registrato e registrato nell'applicazione:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installazione dello strumento MySQL Workbench per la verifica dei dati

1. Installare lo strumento **MySQL Workbench** dalla [pagina dei download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Nell'installazione guidata: scheda **Selezione funzionalità** selezionare **MySQL Workbench** nella sezione **applicazioni** .
3. Avviare l'app e aggiungere una nuova connessione usando i dati della stringa di connessione del database MySQL di Azure creato durante l'accattonaggio di questa esercitazione.
4. Dopo aver stabilito la connessione, esaminare le tabelle **ASP.NET Identity** create in **IdentityMySQLDatabase.**
5. Si noterà che tutte le tabelle ASP.NET Identity richieste vengono create come illustrato nell'immagine seguente:

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Esaminare la tabella **aspnetusers** per l'istanza di per verificare la presenza di voci durante la registrazione di nuovi utenti.

   [Fare clic sull'immagine seguente per espanderla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
