---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione in produzione | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617634"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Distribuzione Web ASP.NET con Visual Studio: distribuzione in produzione

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come configurare un account Microsoft Azure, creare ambienti di gestione temporanea e di produzione e distribuire l'applicazione Web ASP.NET negli ambienti di gestione temporanea e di produzione usando la funzionalità di pubblicazione con un clic di Visual Studio.

Se si preferisce, è possibile eseguire la distribuzione a un provider di hosting di terze parti. La maggior parte delle procedure descritte in questa esercitazione sono le stesse per un provider di hosting o per Azure, tranne per il fatto che ogni provider ha una propria interfaccia utente per la gestione di account e siti Web. È possibile trovare un provider di hosting nella [raccolta di provider](https://www.microsoft.com/web/hosting) nel sito Web Microsoft.com.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla risoluzione dei problemi in questa serie di esercitazioni.

## <a name="get-a-microsoft-azure-account"></a>Ottenere un account di Microsoft Azure

Se non si ha già un account Azure, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Creazione di un ambiente di gestione temporanea

> [!NOTE]
> Poiché questa esercitazione è stata scritta, app Azure servizio ha aggiunto una nuova funzionalità per automatizzare molti dei processi per la creazione di ambienti di gestione temporanea e di produzione. Vedere [configurare gli ambienti di staging per le app Web nel servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Come illustrato nell' [esercitazione distribuire nell'ambiente di testing](deploying-to-iis.md), l'ambiente di test più affidabile è un sito Web del provider di hosting che è proprio come il sito Web di produzione. In molti provider di hosting è necessario valutare i vantaggi di questo problema rispetto a costi aggiuntivi significativi, ma in Azure è possibile creare un'app Web gratuita aggiuntiva come app di staging. È necessario anche un database e la spesa aggiuntiva per tale operazione a scapito del database di produzione sarà di tipo nessuno o minimo. In Azure si paga per la quantità di spazio di archiviazione del database usato anziché per ogni database e la quantità di spazio di archiviazione aggiuntivo che verrà usata nella gestione temporanea sarà minima.

Come illustrato nell' [esercitazione distribuire nell'ambiente di testing](deploying-to-iis.md), in gestione temporanea e produzione verranno distribuiti i due database in un unico database. Per mantenerle separate, il processo è identico, ad eccezione del fatto che si crea un database aggiuntivo per ogni ambiente e si seleziona la stringa di destinazione corretta per ogni database quando si crea il profilo di pubblicazione.

In questa sezione dell'esercitazione si creerà un'app Web e un database da usare per l'ambiente di gestione temporanea e si eseguirà la distribuzione in staging e test prima della creazione e della distribuzione nell'ambiente di produzione.

> [!NOTE]
> I passaggi seguenti illustrano come creare un'app Web nel servizio app Azure tramite il portale di gestione di Azure. Nella versione più recente di Azure SDK, è anche possibile eseguire questa operazione senza uscire da Visual Studio, usando Esplora server. In Visual Studio 2013, è anche possibile creare un'app Web direttamente dalla finestra di dialogo pubblica. Per altre informazioni, vedere [creare un'app web ASP.NET nel servizio app Azure.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/)fare clic su **siti Web**, quindi fare clic su **nuovo**.
2. Fare clic su **sito Web**, quindi su **creazione personalizzata**.

    Viene visualizzata la procedura guidata **nuovo sito web-creazione personalizzata** . La procedura guidata **creazione personalizzata** consente di creare un sito Web e un database nello stesso momento.
3. Nel passaggio **Crea sito Web** della procedura guidata immettere una stringa nella casella **URL** da usare come URL univoco per l'ambiente di gestione temporanea dell'applicazione. Immettere, ad esempio, ContosoUniversity-staging123 (compresi i numeri casuali alla fine per renderlo univoco nel caso in cui venga creato ContosoUniversity-staging).

    L'URL completo sarà costituito da quanto immesso qui, oltre al suffisso visualizzato accanto alla casella di testo.
4. Nell'elenco a discesa **area** scegliere l'area più vicina.

    Questa impostazione specifica i data center in cui verrà eseguita l'app Web.
5. Nell'elenco a discesa **database** scegliere **Crea un nuovo database SQL**.
6. Nella casella **nome stringa di connessione database** lasciare il valore predefinito *DefaultConnection*.
7. Fare clic sulla freccia che punta a destra nella parte inferiore della casella.

    Nella figura seguente viene illustrata la finestra di dialogo **Crea sito Web** con valori di esempio. L'URL e l'area immessi saranno diversi.

    ![Passaggio Crea sito Web](deploying-to-production/_static/image1.png)

    La procedura guidata consente di passare al passaggio **specificare le impostazioni del database** .
8. Nella casella **nome** immettere *ContosoUniversity* più un numero casuale per renderlo univoco, ad esempio *ContosoUniversity123*.
9. Nella casella **Server** selezionare **nuovo server di database SQL**.
10. Immettere il nome e la password di un amministratore.

    Non immettere un nome e una password esistenti qui. Si sta immettendo un nuovo nome e una password che si sta definendo ora per usarli in seguito quando si accede al database.
11. Nella casella **Region (area** ) scegliere la stessa area selezionata per l'app Web.

    Mantenere il server Web e il server di database nella stessa area offre le migliori prestazioni e riduce al minimo le spese.
12. Fare clic sul segno di spunta nella parte inferiore della casella per indicare che l'operazione è terminata.

    Nella figura seguente viene illustrata la finestra di dialogo **specifica impostazioni database** con valori di esempio. I valori immessi potrebbero essere diversi.

    ![Passaggio impostazioni database del nuovo sito Web-Creazione guidata database](deploying-to-production/_static/image2.png)

    Il portale di gestione torna alla pagina siti Web e la colonna **stato** indica che è in corso la creazione dell'app Web. Dopo un periodo di tempo (in genere inferiore a un minuto), nella colonna **stato** viene indicato che l'app Web è stata creata correttamente. Sulla barra di spostamento a sinistra, il numero di app Web disponibili nell'account viene visualizzato accanto all'icona **siti Web** e il numero di database viene visualizzato accanto all'icona **database SQL** .

    ![Pagina siti Web di portale di gestione, sito Web creato](deploying-to-production/_static/image3.png)

    Il nome dell'app Web sarà diverso dall'app di esempio illustrata nell'illustrazione.

## <a name="deploy-the-application-to-staging"></a>Distribuire l'applicazione in gestione temporanea

Ora che è stata creata un'app Web e un database per l'ambiente di gestione temporanea, è possibile distribuirvi il progetto.

> [!NOTE]
> Queste istruzioni illustrano come creare un profilo di pubblicazione scaricando un file con *estensione publishsettings* , che funziona non solo per Azure, ma anche per i provider di hosting di terze parti. La versione più recente di Azure SDK consente anche di connettersi direttamente ad Azure da Visual Studio e di scegliere da un elenco di app Web disponibili nell'account Azure. In Visual Studio 2013, è possibile accedere ad Azure dalla finestra di dialogo di **pubblicazione Web** o dalla finestra di **Esplora server** . Per altre informazioni, vedere [creare un'app web ASP.NET nel servizio app Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Scaricare il file con estensione publishsettings

1. Fare clic sul nome dell'app Web appena creata.

    ![Fare clic sul sito per passare al dashboard](deploying-to-production/_static/image4.png)
2. In **Quick Glance** nella scheda **Dashboard** fare clic su **Scarica profilo di pubblicazione**.

    ![Collegamento Scarica profilo di pubblicazione](deploying-to-production/_static/image5.png)

    Questo passaggio consente di scaricare un file che contiene tutte le impostazioni necessarie per distribuire un'applicazione nell'app Web. Il file verrà importato in Visual Studio in modo da non dover immettere manualmente queste informazioni.
3. Salvare il file con *estensione publishsettings* in una cartella a cui è possibile accedere da Visual Studio.

    ![salvataggio del file con estensione publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sicurezza: il file *. publishsettings* contiene le credenziali (non codificate) usate per amministrare le sottoscrizioni e i servizi di Azure. La procedura consigliata di sicurezza per questo file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine, ad esempio nella cartella Raccolte\documenti, e quindi eliminarlo al termine dell'importazione. Un utente malintenzionato che accede al file *publishsettings* può modificare, creare ed eliminare i servizi di Azure.

### <a name="create-a-publish-profile"></a>Creare un profilo di pubblicazione

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto ContosoUniversity in **Esplora soluzioni** e scegliere **pubblica** dal menu di scelta rapida.

    Verrà visualizzata la procedura guidata **Pubblica sito Web** .
2. Fare clic sulla scheda **profilo** .
3. Fare clic su **Importa**.
4. Passare al file con *estensione publishsettings* scaricato in precedenza e quindi fare clic su **Apri**.

    ![Finestra di dialogo Importa impostazioni di pubblicazione](deploying-to-production/_static/image7.png)
5. Nella scheda **connessione** fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.

    Quando la connessione è stata convalidata, accanto al pulsante **convalida connessione** viene visualizzato un segno di spunta verde.

    Per alcuni provider di hosting, quando si fa clic su **convalida connessione**, potrebbe essere visualizzata una finestra di dialogo di **errore del certificato** . In tal caso, verificare che il nome del server sia quello previsto. Se il nome del server è corretto, selezionare **Salva questo certificato per le sessioni future di Visual Studio** e fare clic su **accetta**. Questo errore indica che il provider di hosting ha scelto di evitare il costo di acquisto di un certificato SSL per l'URL in cui si esegue la distribuzione. Se si preferisce stabilire una connessione sicura usando un certificato valido, contattare il provider di hosting.
6. Scegliere **Avanti**.

    ![icona connessione riuscita e pulsante Avanti nella scheda connessione](deploying-to-production/_static/image8.png)
7. Nella scheda **Impostazioni** espandere Opzioni di **pubblicazione file**e quindi selezionare **Escludi file dalla cartella app\_data**.

    Per informazioni sulle altre opzioni in **Opzioni di pubblicazione file**, vedere l'esercitazione [Deploying to IIS](deploying-to-iis.md) . Lo screenshot che mostra il risultato di questo passaggio e i passaggi di configurazione del database seguenti si trova alla fine dei passaggi di configurazione del database.
8. In **DefaultConnection** nella sezione **database** configurare la distribuzione del database per il database delle appartenenze.
9. 1. Selezionare **Aggiorna database**.

        La casella **stringa di connessione remota** immediatamente sotto **DefaultConnection** viene compilata con la stringa di connessione del file. publishsettings. La stringa di connessione include SQL Server credenziali, che sono archiviate in testo normale nel file con *estensione pubxml* . Se si preferisce non archiviarli in modo permanente, è possibile rimuoverli dal profilo di pubblicazione dopo che il database è stato distribuito e archiviarli in Azure. Per ulteriori informazioni, vedere [come garantire la protezione delle stringhe di connessione al database ASP.NET durante la distribuzione in Azure dall'origine](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) nel Blog di Scott hanselr.
      2. Fare clic su **Configura aggiornamenti database**.
      3. Nella finestra di dialogo **Configura aggiornamenti database** fare clic su **Aggiungi script SQL**.
      4. Nella casella **Aggiungi script SQL** passare allo script *ASPNET-data-prod. SQL* salvato in precedenza nella cartella della soluzione, quindi fare clic su **Apri**.
      5. Chiudere la finestra di dialogo **Configura aggiornamenti database** .
10. In **schoolContext** nella sezione **database** Selezionare **Esegui migrazioni Code First (eseguito all'avvio dell'applicazione)** .

    In Visual Studio viene visualizzato **esegui migrazioni Code First** anziché **aggiorna database** per `DbContext` classi. Se si desidera utilizzare il provider dbDacFx anziché le migrazioni per distribuire un database a cui si accede tramite una classe `DbContext`, vedere [ricerca per categorie distribuire un database Code First senza migrazioni?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nelle domande frequenti sulla distribuzione Web per Visual Studio e ASP.NET su MSDN.

    La scheda **Impostazioni** ha ora un aspetto simile all'esempio seguente:

    ![Scheda impostazioni per la gestione temporanea](deploying-to-production/_static/image9.png)
11. Per salvare il profilo e rinominarlo in *staging*, attenersi alla procedura seguente:

    1. Fare clic sulla scheda **profilo** , quindi fare clic su **Gestisci profili**.
    2. L'importazione ha creato due nuovi profili, uno per FTP e uno per Distribuzione Web. È stato configurato il profilo di Distribuzione Web: rinominare questo profilo in *staging*.

        ![Rinomina profilo in gestione temporanea](deploying-to-production/_static/image10.png)
    3. Chiudere la finestra di dialogo **modifica profili di pubblicazione Web** .
    4. Chiudere la procedura guidata **Pubblica sito Web** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurare una trasformazione del profilo di pubblicazione per l'indicatore dell'ambiente

> [!NOTE]
> In questa sezione viene illustrato come configurare una trasformazione Web. config per l'indicatore dell'ambiente. Poiché l'indicatore si trova nell'elemento `<appSettings>`, è possibile specificare un'altra alternativa per specificare la trasformazione quando si esegue la distribuzione in app Azure servizio. Per altre informazioni, vedere [specifica delle impostazioni di Web. config in Azure](web-config-transformations.md#watransforms).

1. In **Esplora soluzioni**espandere **Proprietà**, quindi espandere **PublishProfiles**.
2. Fare clic con il pulsante destro del mouse su *staging. pubxml*, quindi scegliere **Aggiungi trasformazione configurazione**.

    ![Aggiungi trasformazione configurazione per la gestione temporanea](deploying-to-production/_static/image11.png)

    Visual Studio crea il file di trasformazione *Web. staging. config* e lo apre.
3. Nel file di trasformazione *Web. staging. config* inserire il codice seguente subito dopo il tag di apertura della `configuration`.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando si usa il profilo di pubblicazione di gestione temporanea, questa trasformazione imposta l'indicatore dell'ambiente su "prod". Nell'app Web distribuita non verrà visualizzato alcun suffisso, ad esempio "(dev)" o "(test)" dopo l'intestazione "Contoso University" H1.
4. Fare clic con il pulsante destro del mouse sul file *Web. staging. config* e fare clic su **Anteprima trasformazione** per assicurarsi che la trasformazione codificata produca le modifiche previste.

    Nella finestra di **Anteprima di Web. config** viene visualizzato il risultato dell'applicazione delle trasformazioni *Web. Release. config* e delle trasformazioni di *Web. staging. config* .

### <a name="prevent-public-use-of-the-test-app"></a>Impedisci l'uso pubblico dell'app di test

Una considerazione importante per l'app di staging è che sarà disponibile su Internet, ma non si vuole che il pubblico lo usi. Per ridurre al minimo la probabilità che gli utenti trovino e utilizzino questa funzione, è possibile usare uno o più dei metodi seguenti:

- Impostare le regole del firewall che consentono l'accesso all'app di staging solo da indirizzi IP usati per testare la gestione temporanea.
- Usare un URL offuscato che sarebbe impossibile indovinare.
- Creare un file *robots. txt* per assicurarsi che i motori di ricerca non effettueranno la ricerca per indicizzazione nell'app di test e i collegamenti ai report nei risultati della ricerca.

Il primo di questi metodi è il più efficace, ma non viene trattato in questa esercitazione, perché richiede la distribuzione in un servizio cloud di Azure anziché app Azure servizio. Per altre informazioni sui servizi cloud e sulle restrizioni IP in Azure, vedere [Opzioni di hosting di calcolo fornite da Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [bloccare l'accesso a un ruolo Web da parte di indirizzi IP specifici](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se si esegue la distribuzione in un provider di hosting di terze parti, contattare il provider per scoprire come implementare le restrizioni IP.

Per questa esercitazione verrà creato un file *robots. txt* .

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **Aggiungi nuovo elemento**.
2. Creare un nuovo **file di testo** denominato *robots. txt*e inserire il testo seguente:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    La riga `User-agent` indica ai motori di ricerca che le regole del file si applicano a tutti i crawler web (robot) del motore di ricerca e la riga `Disallow` specifica che non deve essere eseguita la ricerca per indicizzazione nelle pagine del sito.

    Si vuole che i motori di ricerca cataloghino l'app di produzione, quindi è necessario escludere questo file dalla distribuzione di produzione. A tale scopo, è necessario configurare un'impostazione nel profilo di pubblicazione di produzione al momento della creazione.

### <a name="deploy-to-staging"></a>Distribuisci in staging

1. Per aprire la procedura guidata **Pubblica sito Web** , fare clic con il pulsante destro del mouse sul progetto Contoso University e scegliere **pubblica**.
2. Verificare che il profilo di **gestione temporanea** sia selezionato.
3. Fare clic su **Pubblica**.

    Nella finestra **output** vengono visualizzate le azioni di distribuzione eseguite e viene segnalato il corretto completamento della distribuzione. Il browser predefinito si apre automaticamente all'URL dell'app Web distribuita.

## <a name="test-in-the-staging-environment"></a>Test nell'ambiente di gestione temporanea

Si noti che l'indicatore di ambiente è assente (non è presente alcun "(test)" o "(dev)" dopo l'intestazione H1, che indica che la trasformazione *Web. config* per l'indicatore di ambiente ha avuto esito positivo.

![Gestione temporanea della Home page](deploying-to-production/_static/image12.png)

Eseguire la pagina **students** per verificare che il database distribuito non disponga di studenti.

Eseguire la pagina **insegnanti** per verificare che Code First il seeding del database con i dati dell'insegnante:

Selezionare **Aggiungi studenti** dal menu **students (studenti** ), aggiungere uno studente, quindi visualizzare il nuovo studente nella pagina **students (studenti** ) per verificare che sia possibile scrivere correttamente nel database.

Dalla pagina **corsi** fare clic su **Aggiorna crediti**. La pagina di **aggiornamento crediti** richiede autorizzazioni di amministratore, pertanto viene visualizzata la pagina **di accesso** . Immettere le credenziali dell'account amministratore create in precedenza ("admin" e "prodpwd"). Viene visualizzata la pagina **Update Credits** , che verifica che l'account Administrator creato nell'esercitazione precedente sia stato distribuito correttamente nell'ambiente di test.

Richiedere un URL non valido per generare un errore che verrà rilevato da ELMAH, quindi richiedere la segnalazione errori ELMAH. Se si esegue la distribuzione in un provider di hosting di terze parti, probabilmente si noterà che il report è vuoto per lo stesso motivo per cui era vuoto nell'esercitazione precedente. È necessario utilizzare gli strumenti di gestione degli account del provider di hosting per configurare le autorizzazioni per le cartelle per consentire a ELMAH di scrivere nella cartella dei log.

L'applicazione creata viene ora eseguita nel cloud in un'app Web simile a quella che verrà usata per la produzione. Poiché tutto funziona correttamente, il passaggio successivo consiste nella distribuzione in produzione.

## <a name="deploy-to-production"></a>Distribuisci in produzione

Il processo per la creazione di un'app Web di produzione e la distribuzione nell'ambiente di produzione è identico a quello per la gestione temporanea, tranne per il fatto che è necessario escludere la distribuzione di *robots. txt* . A tale scopo, verrà modificato il file del profilo di pubblicazione.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Creare l'ambiente di produzione e il profilo di pubblicazione di produzione

1. Creare l'app Web e il database di produzione in Azure, seguendo la stessa procedura usata per la gestione temporanea.

    Quando si crea il database, è possibile scegliere di inserirlo nello stesso server creato in precedenza oppure creare un nuovo server.
2. Scaricare il file con *estensione publishsettings* .
3. Creare il profilo di pubblicazione importando il file Production *. publishsettings* , seguendo la stessa procedura usata per la gestione temporanea.

    Non dimenticare di configurare lo script di distribuzione dei dati in **DefaultConnection** nella sezione **database** della scheda **Impostazioni** .
4. Rinominare il profilo di pubblicazione in *produzione*.
5. Configurare una trasformazione del profilo di pubblicazione per l'indicatore dell'ambiente, seguendo la stessa procedura usata per la gestione temporanea.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Modificare il file con estensione pubxml per escludere robots. txt

I file del profilo di pubblicazione sono denominati &lt;ProfileName&gt; *. pubxml* e si trovano nella cartella *PublishProfiles* . La *cartella PublishProfiles* si trova nella cartella *Proprietà* di un C# progetto di applicazione Web, nella cartella *progetto* in un progetto di applicazione Web VB o nella cartella *app\_data* in un progetto di app Web. Ogni file *. pubxml* contiene impostazioni che si applicano a un solo profilo di pubblicazione. I valori immessi nella procedura guidata Pubblica sito Web vengono archiviati in questi file ed è possibile modificarli per creare o modificare le impostazioni che non sono rese disponibili nell'interfaccia utente di Visual Studio.

Per impostazione predefinita, i file con *estensione pubxml* sono inclusi nel progetto quando si crea un profilo di pubblicazione, ma è possibile escluderli dal progetto e verranno comunque usati da Visual Studio. Visual Studio cerca i file con *estensione pubxml* nella cartella *PublishProfiles* , indipendentemente dal fatto che siano inclusi nel progetto.

Per ogni file con estensione *pubxml* è presente un file *. pubxml. User* . Il file con *estensione pubxml. User* contiene la password crittografata se è stata selezionata l'opzione **Salva password** e per impostazione predefinita è esclusa dal progetto.

Un file *. pubxml* contiene le impostazioni relative a un profilo di pubblicazione specifico. Se si desidera configurare le impostazioni che si applicano a tutti i profili, è possibile creare un file con estensione *WPP. targets* . Il processo di compilazione importa questi file nel file di progetto con *estensione csproj* o *VBPROJ* , quindi la maggior parte delle impostazioni che è possibile configurare nel file di progetto può essere configurata in questi file. Per altre informazioni sui file con estensione *pubxml* e *WPP. targets* , vedere [procedura: modificare le impostazioni di distribuzione nei file del profilo di pubblicazione (con estensione pubxml) e il file con estensione WPP. targets nei progetti Web di Visual Studio](https://msdn.microsoft.com/library/ff398069.aspx).

1. In **Esplora soluzioni**espandere **Proprietà** ed espandere **PublishProfiles**.
2. Fare clic con il pulsante destro del mouse su *Production. pubxml* e scegliere **Apri**.

    ![Aprire il file con estensione pubxml](deploying-to-production/_static/image13.png)
3. Fare clic con il pulsante destro del mouse su *Production. pubxml* e scegliere **Apri**.
4. Aggiungere le righe seguenti immediatamente prima dell'elemento di chiusura `PropertyGroup`:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Il file con estensione pubxml è ora simile all'esempio seguente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Per ulteriori informazioni su come escludere file e cartelle, vedere è [possibile escludere specifici file o cartelle dalla distribuzione?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) nelle **domande frequenti sulla distribuzione Web per Visual Studio e ASP.NET** su MSDN.

### <a name="deploy-to-production"></a>Distribuisci in produzione

1. Aprire la procedura guidata **Pubblica sito Web** assicurarsi che sia selezionato il profilo di pubblicazione di **produzione** , quindi fare clic su **Avvia anteprima** nella scheda **Anteprima** per verificare che il file *robots. txt* non venga copiato nell'app di produzione.

    ![Anteprima dei file da pubblicare in produzione](deploying-to-production/_static/image14.png)

    Esaminare l'elenco dei file che verranno copiati. Si noterà che tutti i file con estensione *CS* , inclusi i file con *estensione aspx.cs*, *aspx.designer.cs*, *master.cs*e *master.designer.cs* , vengono omessi. Tutto questo codice è stato compilato nei file *ContosoUniversity. dll* e *ContosoUniversity. pdb* disponibili nella cartella *bin* . Poiché è necessaria solo la *dll* per eseguire l'applicazione ed è stato specificato in precedenza che devono essere distribuiti solo i file necessari per eseguire l'applicazione, nessun file con *estensione cs* è stato copiato nell'ambiente di destinazione. La cartella *obj* e i file *ContosoUniversity. csproj* e *. csproj. User* vengono omessi per lo stesso motivo.

    Fare clic su **pubblica** per eseguire la distribuzione nell'ambiente di produzione.
2. Eseguire il test nell'ambiente di produzione, seguendo la stessa procedura usata per la gestione temporanea.

    Tutto è identico alla gestione temporanea, ad eccezione dell'URL e dell'assenza del file *robots. txt* .

## <a name="summary"></a>Riepilogo

L'app Web è stata distribuita e testata correttamente ed è disponibile pubblicamente su Internet.

![Produzione Home page](deploying-to-production/_static/image15.png)

Nell'esercitazione successiva si aggiornerà il codice dell'applicazione e si distribuirà la modifica negli ambienti di test, gestione temporanea e produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione, è necessario implementare un piano di ripristino. Ovvero, è necessario eseguire periodicamente il backup dei database dall'app di produzione in una posizione di archiviazione sicura ed è necessario mantenere diverse generazioni di tali backup. Quando si aggiorna il database, è consigliabile eseguire una copia di backup immediatamente prima della modifica. Quindi, se si commette un errore e non lo si rileva fino a quando non viene distribuito nell'ambiente di produzione, sarà comunque possibile ripristinare il database allo stato in cui si trovava prima che venisse danneggiato. Per altre informazioni, vedere [backup e ripristino del database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> In questa esercitazione l'edizione SQL Server in cui si esegue la distribuzione è il database SQL di Azure. Anche se il processo di distribuzione è simile ad altre edizioni di SQL Server, un'applicazione di produzione reale potrebbe richiedere codice speciale per il database SQL di Azure in alcuni scenari. Per altre informazioni, vedere [uso del database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [scelta tra SQL Server e il database SQL di Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Precedente](setting-folder-permissions.md)
> [Successivo](deploying-a-code-update.md)
