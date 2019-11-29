---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: trasformazioni di file Web. config-3 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: fe71e6cfb0f4c5f1d99b326e9d90edb6c8c5feee
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600512"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: trasformazioni di file Web. config-3 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

In questa esercitazione viene illustrato come automatizzare il processo di modifica del file *Web. config* quando lo si distribuisce in ambienti di destinazione diversi. Per la maggior parte delle applicazioni sono presenti impostazioni nel file *Web. config* che devono essere diverse quando si distribuisce l'applicazione. L'automazione del processo di modifica di queste modifiche impedisce di eseguire manualmente le modifiche ogni volta che si distribuisce, il che sarebbe noioso e soggetto a errori.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Trasformazioni di Web. config rispetto ai parametri di Distribuzione Web

Esistono due modi per automatizzare il processo di modifica delle impostazioni del file *Web. config* : le [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e i [parametri del distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). Un file di trasformazione *Web. config* contiene markup XML che specifica come modificare il file *Web. config* quando viene distribuito. È possibile specificare modifiche diverse per configurazioni di compilazione specifiche e per profili di pubblicazione specifici. Le configurazioni di compilazione predefinite sono debug e release ed è possibile creare configurazioni di compilazione personalizzate. Un profilo di pubblicazione corrisponde in genere a un ambiente di destinazione. Per ulteriori informazioni sui profili di pubblicazione, vedere l'esercitazione [distribuzione in IIS come ambiente di test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

È possibile utilizzare i parametri Distribuzione Web per specificare molti tipi diversi di impostazioni che devono essere configurate durante la distribuzione, incluse le impostazioni presenti nei file *Web. config* . Quando viene utilizzato per specificare le modifiche al file *Web. config* , i parametri distribuzione Web sono più complessi da configurare, ma sono utili quando non si conosce il valore da impostare fino a quando non si esegue la distribuzione. In un ambiente aziendale, ad esempio, è possibile creare un *pacchetto di distribuzione* e assegnarlo a una persona del reparto IT per l'installazione in produzione e tale utente deve essere in grado di immettere le stringhe di connessione o le password che non si conoscono.

Per lo scenario in cui viene illustrata questa esercitazione, si conoscono tutti gli elementi che devono essere eseguiti nel file *Web. config* , pertanto non è necessario utilizzare distribuzione Web parametri. Verranno configurate alcune trasformazioni che variano a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione usato.

## <a name="creating-transformation-files-for-publish-profiles"></a>Creazione di file di trasformazione per i profili di pubblicazione

In **Esplora soluzioni**espandere *Web. config* per visualizzare i file di trasformazione *Web. debug. config* e *Web. Release. config* che vengono creati per impostazione predefinita per le due configurazioni di compilazione predefinite.

![Web. config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

È possibile creare file di trasformazione per le configurazioni di compilazione personalizzate facendo clic con il pulsante destro del mouse sul file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida, ma per questa esercitazione non è necessario eseguire questa operazione.

Sono necessari altri due file di trasformazione, per configurare le modifiche correlate alla destinazione di distribuzione anziché alla configurazione della build. Un esempio tipico di questo tipo di impostazione è un endpoint WCF diverso per i test rispetto alla produzione. Nelle esercitazioni successive verranno creati profili di pubblicazione denominati test e produzione, quindi è necessario un file *Web. test. config* e un file *Web. Production. config* .

È necessario creare manualmente i file di trasformazione collegati ai profili di pubblicazione. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e selezionare **Apri cartella in Esplora risorse**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

In **Esplora risorse**selezionare il file *Web. Release. config* , copiare il file e quindi incollarne due copie. Rinominare le copie *Web. Production. config* e *Web. test. config*, quindi chiudere **Esplora risorse**.

In **Esplora soluzioni**fare clic su **Aggiorna** per visualizzare i nuovi file.

Selezionare i nuovi file, fare clic con il pulsante destro del mouse, quindi scegliere **Includi nel progetto** dal menu di scelta rapida.

![Inclusione di file di configurazione di test e produzione nel progetto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Per impedire che questi file vengano distribuiti, selezionarli in **Esplora soluzioni**, quindi nella finestra **Proprietà** modificare la proprietà **azione di compilazione** da **contenuto** a **nessuno**. La distribuzione dei file di trasformazione basati sulle configurazioni di compilazione viene impedita automaticamente.

A questo punto è possibile immettere le trasformazioni di *Web. config* nei file di trasformazione *Web. config* .

## <a name="limiting-error-log-access-to-administrators"></a>Limitazione dell'accesso ai log degli errori agli amministratori

Se si verifica un errore durante l'esecuzione dell'applicazione, l'applicazione visualizza una pagina di errore generica al posto della pagina di errore generata dal sistema e usa il [pacchetto NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) per la registrazione e la creazione di report degli errori. L'elemento `customErrors` nel file *Web. config* specifica la pagina di errore:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente l'attributo `mode` dell'elemento `customErrors` da "RemoteOnly" a "on" ed eseguire l'applicazione da Visual Studio. Generare un errore richiedendo un URL non valido, ad esempio *Studentsxxx. aspx*. Anziché una pagina di errore "pagina non trovata" generata da IIS, viene visualizzata la pagina *pagina GenericErrorPage. aspx* .

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Per visualizzare il log degli errori, sostituire tutti gli elementi nell'URL dopo il numero di porta con *ELMAH. axd* (per l'esempio nello screenshot, `http://localhost:51130/elmah.axd`) e premere INVIO:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Al termine, non dimenticare di impostare nuovamente l'elemento `customErrors` sulla modalità "RemoteOnly".

Nel computer di sviluppo è consigliabile consentire l'accesso gratuito alla pagina log degli errori, ma in produzione si tratta di un rischio per la sicurezza. Per il sito di produzione, è possibile aggiungere una regola di autorizzazione che limita l'accesso al log degli errori solo agli amministratori configurando una trasformazione nel file *Web. Production. config* .

Aprire *Web. Production. config* e aggiungere un nuovo elemento `location` subito dopo il tag di apertura `configuration`, come illustrato di seguito. Assicurarsi di aggiungere solo l'elemento `location` e non il markup circostante visualizzato solo per fornire un contesto.

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Il valore dell'attributo `Transform` "Insert" comporta l'aggiunta di questo elemento `location` come elemento di pari livello a tutti gli elementi `location` esistenti nel file *Web. config* . Esiste già un elemento `location` che specifica le regole di autorizzazione per la pagina di **aggiornamento crediti** . Quando si esegue il test del sito di produzione dopo la distribuzione, verrà eseguito un test per verificare che questa regola di autorizzazione sia valida.

Non è necessario limitare l'accesso ai log degli errori nell'ambiente di test, pertanto non è necessario aggiungere questo codice al file *Web. test. config* .

> [!NOTE] 
> 
> **Nota sulla sicurezza** Non visualizzare mai i dettagli dell'errore al pubblico in un'applicazione di produzione oppure archiviare le informazioni in un percorso pubblico. Gli utenti malintenzionati possono usare le informazioni sugli errori per individuare le vulnerabilità in un sito. Se si usa ELMAH nella propria applicazione, assicurarsi di esaminare i modi in cui è possibile configurare ELMAH per ridurre al minimo i rischi per la sicurezza. L'esempio ELMAH in questa esercitazione non deve essere considerato una configurazione consigliata. Si tratta di un esempio che è stato scelto per illustrare come gestire una cartella in cui l'applicazione deve essere in grado di creare file.

## <a name="setting-an-environment-indicator"></a>Impostazione di un indicatore di ambiente

Uno scenario comune consiste nel disporre di impostazioni del file *Web. config* che devono essere diverse in ogni ambiente in cui si esegue la distribuzione. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe avere bisogno di un endpoint diverso negli ambienti di test e di produzione. L'applicazione Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile sulle pagine di un sito che indica l'ambiente in cui si trova, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione aggiungerà "(dev)" o "(test)" all'intestazione principale della pagina master del *sito. master* :

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

L'indicatore di ambiente viene omesso quando l'applicazione è in esecuzione nell'ambiente di produzione.

Le pagine Web di Contoso University leggono un valore impostato in `appSettings` nel file *Web. config* per determinare l'ambiente in cui l'applicazione è in esecuzione:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Il valore deve essere "test" nell'ambiente di test e "prod" nell'ambiente di produzione.

Aprire *Web. Production. config* e aggiungere un elemento `appSettings` immediatamente prima del tag di apertura dell'elemento `location` aggiunto in precedenza:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Il valore dell'attributo `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione è modificare i valori di attributo di un elemento esistente nel file *Web. config* . Il valore dell'attributo `xdt:Locator` "Match (Key)" indica che l'elemento da modificare è quello il cui attributo `key` corrisponde all'attributo `key` specificato qui. L'unico altro attributo dell'elemento `add` è `value`ed è ciò che verrà modificato nel file *Web. config* distribuito. Questo codice fa in modo che l'attributo `value` dell'elemento `Environment` `appSettings` sia impostato su "prod" nel file *Web. config* distribuito nell'ambiente di produzione.

Applicare quindi la stessa modifica al file *Web. test. config* , eccetto impostare il `value` su "test" invece di "prod". Al termine, la sezione `appSettings` in *Web. test. config* apparirà come nell'esempio seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Disabilitazione della modalità di debug

Per una build di rilascio, non è necessario abilitare il debug indipendentemente dall'ambiente in cui si esegue la distribuzione. Per impostazione predefinita, il file di trasformazione *Web. Release. config* viene creato automaticamente con il codice che rimuove l'attributo `debug` dall'elemento `compilation`:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

L'attributo `Transform` determina l'omissione dell'attributo `debug` dal file *Web. config* distribuito quando si distribuisce una build di rilascio.

Questa stessa trasformazione si trova nei file di trasformazione di test e di produzione perché sono stati creati copiando il file di trasformazione della versione. Non è necessario duplicarlo, quindi aprire ognuno di questi file, rimuovere l'elemento di **compilazione** e salvare e chiudere ogni file.

## <a name="setting-connection-strings"></a>Impostazione delle stringhe di connessione

Nella maggior parte dei casi non è necessario configurare le trasformazioni delle stringhe di connessione, in quanto è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Viene tuttavia generata un'eccezione quando si distribuisce un database di SQL Server Compact e si utilizza Migrazioni Code First di Entity Framework per aggiornare il database nel server di destinazione. Per questo scenario, è necessario specificare una stringa di connessione aggiuntiva che verrà utilizzata sul server per aggiornare lo schema del database. Per impostare questa trasformazione, aggiungere un elemento **&gt;di&lt;connectionStrings** immediatamente dopo il tag di apertura della **&gt;configurazione del&lt;** di apertura in entrambi i file di trasformazione *Web. test. config* e *Web. Production. config* :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

L'attributo `Transform` specifica che la stringa di connessione verrà aggiunta all'elemento *connectionStrings* nel file *Web. config* distribuito. Il processo di pubblicazione crea automaticamente questa stringa di connessione aggiuntiva se non esiste, ma per impostazione predefinita l'attributo **providerName** viene impostato su `System.Data.SqlClient`, che non funziona per SQL Server Compact. Aggiungendo manualmente la stringa di connessione, si impedisce al processo di distribuzione di creare un elemento della stringa di connessione con il nome del provider errato.

A questo punto sono state specificate tutte le trasformazioni di *Web. config* necessarie per la distribuzione dell'applicazione Contoso University a test e produzione. Nell'esercitazione seguente verranno gestite le attività di configurazione della distribuzione che richiedono l'impostazione delle proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere lo scenario di trasformazione di Web. config nella [mappa del contenuto della distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
