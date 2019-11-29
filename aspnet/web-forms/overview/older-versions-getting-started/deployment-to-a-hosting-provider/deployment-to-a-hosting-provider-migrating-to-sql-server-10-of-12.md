---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrazione a SQL Server-10 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640601"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrazione a SQL Server-10 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come eseguire la migrazione da SQL Server Compact a SQL Server. Questo può essere utile per sfruttare le funzionalità di SQL Server che SQL Server Compact non supporta, ad esempio stored procedure, trigger, viste o replica. Per ulteriori informazioni sulle differenze tra SQL Server Compact e SQL Server, vedere l'esercitazione sulla [distribuzione di SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express rispetto alla SQL Server completa per lo sviluppo

Dopo aver deciso di eseguire l'aggiornamento a SQL Server, potrebbe essere necessario usare SQL Server o SQL Server Express negli ambienti di sviluppo e test. Oltre alle differenze nel supporto degli strumenti e nelle caratteristiche del motore di database, esistono differenze nelle implementazioni del provider tra SQL Server Compact e altre versioni di SQL Server. Queste differenze possono causare lo stesso codice per generare risultati diversi. Se pertanto si decide di tenere SQL Server Compact come database di sviluppo, è necessario testare accuratamente il sito in SQL Server o SQL Server Express in un ambiente di test prima di ogni distribuzione in produzione.

A differenza di SQL Server Compact, SQL Server Express è essenzialmente lo stesso motore di database e usa lo stesso provider .NET come SQL Server completo. Quando si esegue il test con SQL Server Express, è possibile avere la certezza di ottenere gli stessi risultati che si otterranno con SQL Server. È possibile utilizzare la maggior parte degli strumenti di database con SQL Server Express che è possibile utilizzare con SQL Server (un'eccezione significativa [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) e supporta altre funzionalità di SQL Server come stored procedure, viste, trigger e replica. Tuttavia, in genere è necessario utilizzare la SQL Server completa in un sito Web di produzione. SQL Server Express può essere eseguito in un ambiente host condiviso, ma non è stato progettato per questo e molti provider di hosting non lo supportano.

Se si usa Visual Studio 2012, in genere si sceglie SQL Server Express database locale per l'ambiente di sviluppo, perché questo è quello che viene installato per impostazione predefinita con Visual Studio. Tuttavia, il database locale non funziona in IIS, pertanto per l'ambiente di test è necessario utilizzare SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinare i database e mantenerli separati

L'applicazione Contoso University dispone di due database SQL Server Compact: il database delle appartenenze (*ASPNET. sdf*) e il database dell'applicazione (*School. sdf*). Quando si esegue la migrazione, è possibile eseguire la migrazione di questi database a due database distinti o a un singolo database. Potrebbe essere necessario combinarli per facilitare i join del database tra il database dell'applicazione e il database di appartenenza. Il piano di hosting potrebbe anche fornire un motivo per combinarli. Ad esempio, il provider di hosting potrebbe addebitare più database per più database o addirittura non consentire più di un database. Questo è il caso dell'account di hosting Cytanium Lite usato per questa esercitazione, che consente un solo database SQL Server.

In questa esercitazione si eseguirà la migrazione dei due database in questo modo:

- Eseguire la migrazione a due database del database locale nell'ambiente di sviluppo.
- Eseguire la migrazione a due database SQL Server Express nell'ambiente di testing.
- Eseguire la migrazione a un database completo SQL Server combinato nell'ambiente di produzione.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Installazione di SQL Server Express

SQL Server Express viene installato automaticamente per impostazione predefinita con Visual Studio 2010, ma per impostazione predefinita non viene installato con Visual Studio 2012. Per installare SQL Server 2012 Express, fare clic sul collegamento seguente

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Scegliere *ita/x64/SQLEXPR\_x64\_ita. exe* o *enu/x86/SQLEXPR\_x86\_ENU. exe*e nell'installazione guidata accettare le impostazioni predefinite. Per ulteriori informazioni sulle opzioni di installazione, vedere [installare SQL Server 2012 dall'installazione guidata (programma](https://msdn.microsoft.com/library/ms143219.aspx)di installazione).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Creazione di SQL Server Express database per l'ambiente di test

Il passaggio successivo consiste nel creare i database di appartenenza e dell'Istituto di istruzione di ASP.NET.

Dal menu **Visualizza** selezionare **Esplora server** (**Esplora database** in Visual Web Developer), quindi fare clic con il pulsante destro del mouse su **connessioni dati** e scegliere **Crea nuovo SQL Server database**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Nella finestra di dialogo **Crea nuovo database di SQL Server** , immettere ".\SQLEXPRESS" nella **casella nome server** e "ASPNET-test" nella casella **nuovo nome database** , quindi fare clic su **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Seguire la stessa procedura per creare un nuovo database di SQL Server Express School denominato "School-test".

Si sta aggiungendo "test" a questi nomi di database perché in un secondo momento si creerà un'istanza aggiuntiva di ogni database per l'ambiente di sviluppo ed è necessario essere in grado di distinguere i due set di database.

**Esplora server** ora Visualizza i due nuovi database.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Creazione di uno script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione accede al database usando le credenziali del pool di applicazioni predefinito. Tuttavia, per impostazione predefinita, l'identità del pool di applicazioni non dispone dell'autorizzazione per aprire i database. Quindi, è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione viene creato lo script che verrà eseguito in un secondo momento per assicurarsi che l'applicazione possa aprire i database durante l'esecuzione in IIS.

Nella cartella *SolutionFiles* della soluzione creata nell'esercitazione [distribuzione in ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) creare un nuovo file SQL denominato *Grant. SQL*. Copiare i seguenti comandi SQL nel file, quindi salvare e chiudere il file:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server 2008 e con le impostazioni di IIS in Windows 7, così come sono specificate in questa esercitazione. Se si usa una versione diversa di SQL Server o di Windows o se si configura IIS nel computer in modo diverso, potrebbe essere necessario apportare modifiche a questo script. Per ulteriori informazioni sugli script di SQL Server, vedere [documentazione online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Nota sulla sicurezza** Questo script fornisce le autorizzazioni di database\_proprietario all'utente che accede al database in fase di esecuzione, che è ciò che si avrà nell'ambiente di produzione. In alcuni scenari potrebbe essere necessario specificare un utente che disponga di autorizzazioni complete per l'aggiornamento dello schema del database solo per la distribuzione e specificare per la fase di esecuzione un utente diverso che disponga di autorizzazioni solo per la lettura e la scrittura dei dati. Per ulteriori informazioni, vedere **la pagina relativa alla revisione delle modifiche automatiche di Web. config per migrazioni Code First** nella [distribuzione in IIS come ambiente di test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Configurazione della distribuzione di database per l'ambiente di test

Successivamente, verrà configurato Visual Studio in modo che esegua le attività seguenti per ogni database:

- Generare uno script SQL per la creazione della struttura del database di origine (tabelle, colonne, vincoli e così via) nel database di destinazione.
- Genera uno script SQL che inserisce i dati del database di origine nelle tabelle del database di destinazione.
- Eseguire gli script generati e lo script di concessione creato nel database di destinazione.

Aprire la finestra delle **proprietà del progetto** e selezionare la scheda **pacchetto/pubblica SQL** .

Verificare che nell'elenco a discesa **configurazione** sia selezionato **attivo (versione)** o **versione** .

Fare clic su **Abilita questa pagina**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

La scheda **pacchetto/pubblica SQL** è generalmente disabilitata perché specifica un metodo di distribuzione legacy. Per la maggior parte degli scenari, è necessario configurare la distribuzione del database nella procedura guidata **Pubblica sito Web** . La migrazione da SQL Server Compact a SQL Server o SQL Server Express è un caso speciale per cui questo metodo è una scelta ottimale.

Fare clic su **Importa da Web. config**.

![Selecting_Import_from_Web. config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio cerca le stringhe di connessione nel file *Web. config* , ne trova uno per il database di appartenenza e uno per il database School e aggiunge una riga corrispondente a ogni stringa di connessione nella tabella delle **voci del database** . Le stringhe di connessione trovate per i database di SQL Server Compact esistenti e il passaggio successivo consiste nel configurare come e dove distribuire questi database.

Immettere le impostazioni di distribuzione del database nella sezione **Dettagli voce database** sotto la tabella **voci di database** . Le impostazioni visualizzate nella sezione **Dettagli immissione database** riguardano la riga selezionata nella tabella **voci di database** , come illustrato nella figura seguente.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il database delle appartenenze

Selezionare la riga **DefaultConnection-Deployment** nella tabella **voci di database** per configurare le impostazioni che si applicano al database delle appartenenze.

In **stringa di connessione per database di destinazione**immettere una stringa di connessione che punta al nuovo database di appartenenza SQL Server Express. È possibile ottenere la stringa di connessione necessaria dall' **Esplora server**. In **Esplora server**espandere **connessioni dati** e selezionare il database **AspnetTest** , quindi nella finestra **Proprietà** copiare il valore della **stringa di connessione** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

La stessa stringa di connessione viene riprodotta qui:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copiare e incollare la stringa di connessione nella **stringa di connessione per il database di destinazione** nella scheda **pacchetto/pubblica SQL** .

Assicurarsi che sia selezionata l'opzione **Estrai dati e/o schema da un database esistente** . Questo è ciò che fa in modo che gli script SQL vengano generati e eseguiti automaticamente nel database di destinazione.

La **stringa di connessione per il valore del database di origine** viene estratta dal file *Web. config* e punta al database di sviluppo SQL Server Compact. Si tratta del database di origine che verrà usato per generare gli script che verranno eseguiti in un secondo momento nel database di destinazione. Poiché si desidera distribuire la versione di produzione del database, modificare "aspnet-Dev. sdf" in "aspnet-Prod. sdf".

Modificare le **Opzioni di scripting del database** dallo **schema solo** a **schemi e dati**, dal momento che si desidera copiare i dati (account utente e ruoli), nonché la struttura del database.

Per configurare la distribuzione per l'esecuzione degli script di concessione creati in precedenza, è necessario aggiungerli alla sezione **script di database** . Fare clic su **Aggiungi script**e nella finestra di dialogo **Aggiungi script SQL** passare alla cartella in cui è stato archiviato lo script di concessione (si tratta della cartella che contiene il file della soluzione). Selezionare il file denominato *Grant. SQL*e fare clic su **Apri**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Le impostazioni per la riga **DefaultConnection-Deployment** nelle **voci del database** hanno ora un aspetto simile a quello illustrato nella figura seguente:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il database School

Selezionare quindi la riga **schoolContext-Deployment** nella tabella delle **voci del database** per configurare le impostazioni di distribuzione per il database School.

È possibile usare lo stesso metodo usato in precedenza per ottenere la stringa di connessione per il nuovo database di SQL Server Express. Copiare questa stringa di connessione nella **stringa di connessione per il database di destinazione** nella scheda **pacchetto/pubblica SQL** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Assicurarsi che sia selezionata l'opzione **Estrai dati e/o schema da un database esistente** .

La **stringa di connessione per il valore del database di origine** viene estratta dal file *Web. config* e punta al database di sviluppo SQL Server Compact. Modificare "School-Dev. sdf" in "School-Prod. sdf" per distribuire la versione di produzione del database. Non è mai stato creato un file School-Prod. sdf nella cartella app\_data, quindi copiare il file dall'ambiente di test alla cartella app\_data nella cartella del progetto ContosoUniversity in un secondo momento.

Modificare le **Opzioni di scripting del database per lo** **schema e i dati**.

Si vuole anche eseguire lo script per concedere l'autorizzazione di lettura e scrittura per il database all'identità del pool di applicazioni, quindi aggiungere il file di script *Grant. SQL* come è stato fatto per il database delle appartenenze.

Al termine, le impostazioni per la riga **schoolContext-Deployment** nelle voci del **database** sono simili a quelle illustrate nella figura seguente:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Salvare le modifiche apportate alla scheda **pubblicazione/pubblicazione pacchetto SQL** .

Copiare il file *School-prod. sdf* dalla cartella *c:\inetpub\wwwroot\ContosoUniversity\App\_data* alla cartella *app\_data* nel progetto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Specifica della modalità transazionale per lo script Grant

Il processo di distribuzione genera script che distribuiscono lo schema e i dati del database. Per impostazione predefinita, questi script vengono eseguiti in una transazione. Tuttavia, per impostazione predefinita, gli script personalizzati, come gli script di concessione, non vengono eseguiti in una transazione. Se il processo di distribuzione combina le modalità di transazione, è possibile che si ottenga un errore di timeout quando gli script vengono eseguiti durante la distribuzione. In questa sezione si modifica il file di progetto per configurare gli script personalizzati da eseguire in una transazione.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **ContosoUniversity** e scegliere **Scarica progetto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Quindi fare di nuovo clic con il pulsante destro del mouse sul progetto e scegliere **modifica ContosoUniversity. csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Nell'editor di Visual Studio viene visualizzato il contenuto XML del file di progetto. Si noti che sono presenti diversi elementi `PropertyGroup`. Nell'immagine il contenuto degli elementi `PropertyGroup` è stato omesso.

![Finestra Editor file di progetto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Il primo, che non ha un attributo `Condition`, è per le impostazioni che si applicano indipendentemente dalla configurazione della build. Un `PropertyGroup` elemento si applica solo alla configurazione della build di debug (si noti l'attributo `Condition`), uno si applica solo alla configurazione della build di rilascio e uno si applica solo alla configurazione della build di test. All'interno dell'elemento `PropertyGroup` per la configurazione della build di rilascio, verrà visualizzato un elemento `PublishDatabaseSettings` che contiene le impostazioni immesse nella scheda **Pubblicazione/creazione pacchetto SQL** . È presente un elemento `Object` che corrisponde a tutti gli script di concessione specificati (si notino le due istanze di "Grant. SQL"). Per impostazione predefinita, l'attributo `Transacted` dell'elemento `Source` per ogni script di concessione è `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Modificare il valore dell'attributo `Transacted` dell'elemento `Source` in `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Salvare e chiudere il file di progetto, quindi fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Ricarica progetto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Impostazione delle trasformazioni di Web. config per le stringhe di connessione

Le stringhe di connessione per i nuovi database di SQL Express immessi nella scheda **Pubblicazione/creazione pacchetto SQL** vengono utilizzate da distribuzione Web solo per l'aggiornamento del database di destinazione durante la distribuzione. È comunque necessario configurare le trasformazioni di *Web. config* in modo che le stringhe di connessione nel file *Web. config* distribuito puntino ai nuovi database di SQL Server Express. Quando si utilizza la scheda **pacchetto/pubblica SQL** , non è possibile configurare le stringhe di connessione nel profilo di pubblicazione.

Aprire *Web. test. config* e sostituire l'elemento `connectionStrings` con l'elemento `connectionStrings` nell'esempio seguente. Assicurarsi di copiare solo l'elemento connectionStrings, non il codice circostante riportato qui per fornire il contesto.

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Questo codice fa in modo che gli attributi `connectionString` e `providerName` di ogni elemento `add` vengano sostituiti nel file *Web. config* distribuito. Queste stringhe di connessione non sono identiche a quelle immesse nella scheda **pubblicazione/pubblicazione pacchetto SQL** . È stata aggiunta l'impostazione "MultipleActiveResultSets = true" perché è necessaria per il Entity Framework e il provider universali.

## <a name="installing-sql-server-compact"></a>Installazione di SQL Server Compact

Il pacchetto NuGet SqlServerCompact fornisce gli assembly del motore di database SQL Server Compact per l'applicazione Contoso University. Tuttavia, non si tratta dell'applicazione, ma Distribuzione Web che deve essere in grado di leggere i database SQL Server Compact per creare gli script da eseguire nei database SQL Server. Per consentire Distribuzione Web di leggere SQL Server Compact database, installare SQL Server Compact nel computer di sviluppo usando il collegamento seguente: [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Distribuzione nell'ambiente di test

Per eseguire la pubblicazione nell'ambiente di test, è necessario creare un profilo di pubblicazione configurato per utilizzare la scheda pubblicazione **/creazione pacchetto SQL** per la pubblicazione del database anziché le impostazioni del database del profilo di pubblicazione.

Prima di tutto, eliminare il profilo di test esistente.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.

Selezionare la scheda **profilo** .

Fare clic su **Gestisci profili**.

Selezionare **test**, fare clic su **Rimuovi**e quindi su **Chiudi**.

Chiudere la procedura guidata **Pubblica sito Web** per salvare la modifica.

Successivamente, creare un nuovo profilo di test e utilizzarlo per pubblicare il progetto.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.

Selezionare la scheda **profilo** .

Selezionare **&lt;nuova&gt;** dall'elenco a discesa e immettere "test" come nome del profilo.

Nella casella **URL servizio** immettere *localhost*.

Nella casella **sito/applicazione** immettere *Default Web site/ContosoUniversity*.

Nella casella **URL di destinazione** immettere `http://localhost/ContosoUniversity/`.

Scegliere **Avanti**.

Nella scheda **Impostazioni** viene visualizzato un avviso indicante che la scheda **Pubblicazione/creazione pacchetto SQL** è stata configurata ed è possibile eseguirne l'override facendo clic su Abilita nuovi miglioramenti alla pubblicazione del database. Per questa distribuzione non si desidera ignorare le impostazioni della scheda **pubblicazione/pubblicazione pacchetto SQL** , quindi è sufficiente fare clic su **Avanti**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un messaggio nella scheda **Anteprima** indica che non è stato **selezionato alcun database per la pubblicazione**, ma ciò significa solo che la pubblicazione del database non è configurata nel profilo di pubblicazione.

Fare clic su **Pubblica**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio distribuisce l'applicazione e apre il browser all'home page del sito nell'ambiente di test. Eseguire la pagina insegnanti per vedere che visualizza gli stessi dati visualizzati in precedenza. Eseguire la pagina **Aggiungi studenti** , aggiungere un nuovo studente e visualizzare il nuovo studente nella pagina **students (studenti** ). Ciò consente di verificare che sia possibile aggiornare il database. Selezionare la pagina **Update Credits** (è necessario accedere) per verificare che il database delle appartenenze sia stato distribuito e che sia possibile accedervi.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Creazione di un database di SQL Server per l'ambiente di produzione

Ora che è stata eseguita la distribuzione nell'ambiente di test, si è pronti per configurare la distribuzione nell'ambiente di produzione. Si inizia come per l'ambiente di testing creando un database in cui eseguire la distribuzione. Come è possibile ricordare dalla panoramica, il piano di hosting di Cytanium Lite consente un solo database di SQL Server, quindi è necessario configurare un solo database, non due. Tutte le tabelle e i dati dei database di appartenenza e dell'Istituto di istruzione SQL Server Compact verranno distribuiti in un database SQL Server in produzione.

Passare al pannello di controllo di Cytanium in [http://panel.cytanium.com](http://panel.cytanium.com). Mantenere il puntatore del mouse su **database** , quindi fare clic su **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Nella pagina **SQL Server 2008** fare clic su **Crea database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Assegnare un nome al database "School" e fare clic su **Save**. La pagina aggiunge automaticamente il prefisso "contoso", quindi il nome effettivo sarà "contosouSchool".

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Nella stessa pagina fare clic su **Crea utente**. Nei server di Cytanium, invece di usare la sicurezza integrata di Windows e consentire all'identità del pool di applicazioni di aprire il database, verrà creato un utente con l'autorità per aprire il database. Le credenziali dell'utente verranno aggiunte alle stringhe di connessione che vengono inserite nel file *Web. config* di produzione. In questo passaggio vengono create le credenziali.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Compilare i campi obbligatori nella pagina **proprietà utente SQL** :

- Immettere "ContosoUniversityUser" come nome.
- Immettere una password.
- Selezionare **contosouSchool** come database predefinito.
- Selezionare la casella di controllo **contosouSchool** .

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configurazione della distribuzione di database per l'ambiente di produzione

A questo punto si è pronti per configurare le impostazioni di distribuzione del database nella scheda **pubblicazione/pubblicazione pacchetto SQL** , come in precedenza per l'ambiente di test.

Aprire la finestra delle **proprietà del progetto** , selezionare la scheda **pacchetto/pubblica SQL** e assicurarsi che sia selezionata l'opzione **attiva (versione)** o **versione** nell'elenco a discesa **configurazione** .

Quando si configurano le impostazioni di distribuzione per ogni database, la differenza principale tra le operazioni eseguite per gli ambienti di produzione e di test è la modalità di configurazione delle stringhe di connessione. Per l'ambiente di test sono state immesse diverse stringhe di connessione al database di destinazione, ma per l'ambiente di produzione la stringa di connessione di destinazione sarà la stessa per entrambi i database. Questo perché si distribuiscono entrambi i database in un database di produzione.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il database delle appartenenze

Per configurare le impostazioni che si applicano al database di appartenenza, selezionare la riga **DefaultConnection-Deployment** nella tabella **voci di database** .

In **stringa di connessione per database di destinazione**immettere una stringa di connessione che punta al nuovo database di produzione SQL Server appena creato. È possibile ottenere la stringa di connessione dal messaggio di posta elettronica di benvenuto. La parte pertinente del messaggio di posta elettronica contiene la stringa di connessione di esempio seguente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Dopo aver sostituito le tre variabili, la stringa di connessione necessaria è simile a questo esempio:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copiare e incollare la stringa di connessione nella **stringa di connessione per il database di destinazione** nella scheda **pacchetto/pubblica SQL** .

Assicurarsi che **i dati di pull e/o lo schema da un database esistente** siano ancora selezionati e che le **Opzioni di scripting del database** siano ancora **schema e dati**.

Nella casella **script di database** deselezionare la casella di controllo accanto allo script Grant. SQL.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il database School

Selezionare quindi la riga **schoolContext-Deployment** nella tabella delle **voci del database** per configurare le impostazioni del database School.

Copiare la stessa stringa di connessione nella **stringa di connessione per il database di destinazione** copiato nel campo per il database delle appartenenze.

Assicurarsi che **i dati di pull e/o lo schema da un database esistente** siano ancora selezionati e che le **Opzioni di scripting del database** siano ancora **schema e dati**.

Nella casella **script di database** deselezionare la casella di controllo accanto allo script Grant. SQL.

Salvare le modifiche apportate alla scheda **pubblicazione/pubblicazione pacchetto SQL** .

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Impostazione delle trasformazioni di Web. config per le stringhe di connessione nei database di produzione

Successivamente, si configureranno le trasformazioni di *Web. config* in modo che le stringhe di connessione nel file *Web. config* distribuito facciano riferimento al nuovo database di produzione. La stringa di connessione immessa nella scheda **pacchetto/pubblica SQL** per distribuzione Web da usare corrisponde a quella usata dall'applicazione, ad eccezione dell'aggiunta dell'opzione `MultipleResultSets`.

Aprire *Web. Production. config* e sostituire l'elemento `connectionStrings` con un elemento `connectionStrings` simile all'esempio seguente. (Copiare solo l'elemento `connectionStrings`, non i tag circostanti forniti per mostrare il contesto).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

A volte viene visualizzato un Consiglio che indica di crittografare sempre le stringhe di connessione nel file *Web. config* . Questo potrebbe essere appropriato se si stava distribuendo ai server nella rete aziendale. Quando si esegue la distribuzione in un ambiente host condiviso, tuttavia, le procedure di sicurezza del provider di hosting sono attendibili e non è necessario o pratico crittografare le stringhe di connessione.

## <a name="deploying-to-the-production-environment"></a>Distribuzione nell'ambiente di produzione

A questo punto si è pronti per la distribuzione nell'ambiente di produzione. Distribuzione Web leggerà i database SQL Server Compact nella cartella *App\_data* del progetto e creerà di nuovo tutte le tabelle e i dati nel database di SQL Server di produzione. Per eseguire la pubblicazione utilizzando le impostazioni della scheda **Pubblicazione/creazione pacchetto Web** , è necessario creare un nuovo profilo di pubblicazione per la produzione.

Prima di tutto, eliminare il profilo di produzione esistente come il profilo di test precedente.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.

Selezionare la scheda **profilo** .

Fare clic su **Gestisci profili**.

Selezionare **produzione**, fare clic su **Rimuovi**e quindi su **Chiudi**.

Chiudere la procedura guidata **Pubblica sito Web** per salvare la modifica.

Successivamente, creare un nuovo profilo di produzione e utilizzarlo per pubblicare il progetto.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.

Selezionare la scheda **profilo** .

Fare clic su **Importa**e selezionare il file con estensione publishsettings scaricato in precedenza.

Nella scheda **connessione** modificare l'URL di **destinazione** con l'URL temporaneo corretto, che in questo esempio è http://contosouniversity.com.vserver01.cytanium.com.

Rinominare il profilo in produzione. Selezionare la scheda **profilo** e fare clic su **Gestisci profili** a tale scopo.

Chiudere la procedura guidata **Pubblica sito Web** per salvare le modifiche.

In un'applicazione reale in cui il database è stato aggiornato in produzione, è necessario eseguire due passaggi aggiuntivi prima di pubblicare:

1. Caricare l' *app\_offline. htm*, come illustrato nell'esercitazione [Deploying to the production environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .
2. Usare la funzionalità **file Manager** del pannello di controllo Cytanium per copiare i file *ASPNET-prod. sdf* e *School-prod. sdf* dal sito di produzione alla cartella *app\_data* del progetto ContosoUniversity. Ciò garantisce che i dati distribuiti nel nuovo database di SQL Server includano gli ultimi aggiornamenti effettuati dal sito Web di produzione.

Nella barra degli strumenti **pubblica sul Web fare clic su pubblica** , verificare che il profilo di **produzione** sia selezionato, quindi fare clic su **pubblica**.

Se l'app è stata caricata <em>\_offline. htm</em> prima della pubblicazione, è necessario usare l'utilità <strong>file Manager</strong> nel pannello di controllo Cytanium per eliminare l' <em>app\_offline.</em> htm prima di eseguire il test. È anche possibile eliminare contemporaneamente i file <em>SDF</em> dalla cartella <em>app\_data</em> .

È ora possibile aprire un browser e passare all'URL del sito pubblico per testare l'applicazione nello stesso modo in cui è stato eseguito dopo la distribuzione nell'ambiente di test.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Passare a SQL Server Express database locale nello sviluppo

Come illustrato nella panoramica, in genere è preferibile usare lo stesso motore di database in fase di sviluppo da usare per il test e la produzione. Tenere presente che il vantaggio dell'utilizzo di SQL Server Express in fase di sviluppo è che il database funziona nello stesso modo negli ambienti di sviluppo, test e produzione. In questa sezione verrà configurato il progetto ContosoUniversity per l'uso di SQL Server Express database locale quando si esegue l'applicazione da Visual Studio.

Il modo più semplice per eseguire questa migrazione è consentire a Code First e al sistema di appartenenza di creare i nuovi database di sviluppo. L'utilizzo di questo metodo per eseguire la migrazione richiede tre passaggi:

1. Modificare le stringhe di connessione per specificare nuovi database del database locale di SQL Express.
2. Eseguire lo strumento Amministrazione sito Web per creare un utente amministratore. Verrà creato il database delle appartenenze.
3. Usare il comando Migrazioni Code First Update-database per creare e inizializzare il database dell'applicazione.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aggiornamento delle stringhe di connessione nel file Web. config

Aprire il file *Web. config* e sostituire l'elemento `connectionStrings` con il codice seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Creazione del database di appartenenza

In **Esplora soluzioni**selezionare il progetto ContosoUniversity e quindi fare clic su **configurazione ASP.NET** nel menu **progetto** .

Fare clic sulla scheda Sicurezza.

Fare clic su **Crea o Gestisci ruoli**, quindi creare un ruolo di **amministratore** .

Tornare alla scheda sicurezza.

Fare clic su **Crea utente**, quindi selezionare la casella di controllo **amministratore** e creare un utente denominato admin.

Chiudere lo **strumento Amministrazione sito Web**.

### <a name="creating-the-school-database"></a>Creazione del database School

Aprire la finestra console di gestione pacchetti.

Nell'elenco a discesa **progetto predefinito** selezionare il progetto CONTOSOUNIVERSITY. dal.

Immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrazioni Code First applica la migrazione iniziale che crea il database e quindi applica la migrazione AddBirthDate, quindi esegue il metodo Seed.

Eseguire il sito premendo CTRL-F5. Come per gli ambienti di test e di produzione, eseguire la pagina **Aggiungi studenti** , aggiungere un nuovo studente, quindi visualizzare il nuovo studente nella pagina **students (studenti** ). In questo modo si verifica che il database School sia stato creato e inizializzato e che l'utente disponga dell'accesso in lettura e scrittura.

Selezionare la pagina di **aggiornamento crediti** e accedere per verificare che il database di appartenenza sia stato distribuito e che sia possibile accedervi. Se non è stata eseguita la migrazione degli account utente, creare un account amministratore e quindi selezionare la pagina **crediti di aggiornamento** per verificarne il funzionamento.

## <a name="cleaning-up-sql-server-compact-files"></a>Pulizia dei file di SQL Server Compact

Non sono più necessari i file e i pacchetti NuGet inclusi per supportare SQL Server Compact. Se si desidera (questo passaggio non è obbligatorio), è possibile eliminare i file e i riferimenti non necessari.

In **Esplora soluzioni**eliminare i file *con estensione sdf* dalla cartella *app\_data* e dalle cartelle *amd64* e *x86* dalla cartella *bin* .

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione, non su uno dei progetti, quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.

Nel riquadro sinistro della finestra di dialogo **Gestisci pacchetti NuGet** selezionare **pacchetti installati**.

Selezionare il pacchetto **EntityFramework. SqlServerCompact** e fare clic su **Gestisci**.

Nella finestra di dialogo **Seleziona progetti** è selezionato entrambi i progetti. Per disinstallare il pacchetto in entrambi i progetti, deselezionare entrambe le caselle di controllo, quindi fare clic su **OK**.

Nella finestra di dialogo in cui viene chiesto se si desidera disinstallare anche i pacchetti dipendenti, fare clic su No. Uno di questi è il pacchetto di Entity Framework che è necessario gestire.

Seguire la stessa procedura per disinstallare il pacchetto **SqlServerCompact** . I pacchetti devono essere disinstallati in questo ordine perché il pacchetto **EntityFramework. SqlServerCompact** dipende dal pacchetto **SqlServerCompact** .

La migrazione a SQL Server Express e SQL Server completa è stata completata. Nell'esercitazione successiva verrà apportata una modifica a un altro database e verrà illustrato come distribuire le modifiche al database quando i database di test e produzione utilizzano SQL Server Express e SQL Server completi.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
