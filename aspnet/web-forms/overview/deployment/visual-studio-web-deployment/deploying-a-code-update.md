---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567403"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del codice

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, il lavoro di manutenzione e sviluppo del sito Web continua e, prima di lungo, sarà necessario distribuire un aggiornamento. Questa esercitazione illustra il processo di distribuzione di un aggiornamento nel codice dell'applicazione. L'aggiornamento implementato e distribuito in questa esercitazione non implica una modifica del database. verranno visualizzate le differenze relative alla distribuzione di una modifica del database nell'esercitazione successiva.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="make-a-code-change"></a>Effettuare una modifica del codice

Come semplice esempio di aggiornamento dell'applicazione, si aggiungerà alla pagina degli **insegnanti** un elenco di corsi insegnato dall'insegnante selezionato.

Se si esegue la pagina **Instructors (insegnanti** ), si noterà che nella griglia sono presenti collegamenti **selezionati** , ma che non eseguono alcuna operazione oltre a rendere grigio lo sfondo della riga.

![Pagina degli insegnanti con selezione](deploying-a-code-update/_static/image1.png)

A questo punto si aggiungerà il codice che viene eseguito quando si fa clic sul collegamento **Seleziona** e viene visualizzato un elenco dei corsi insegnati dall'insegnante selezionato.

1. In *Instructors. aspx*aggiungere il markup seguente subito dopo il controllo `Label` **ErrorMessageLabel** :

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Eseguire la pagina e selezionare un insegnante. Viene visualizzato un elenco dei corsi tenuti da tale insegnante.

    ![Pagina relativa agli insegnanti con corsi insegnati](deploying-a-code-update/_static/image2.png)
3. Chiudere il browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Distribuire l'aggiornamento del codice nell'ambiente di test

Prima di poter usare i profili di pubblicazione per eseguire la distribuzione in test, gestione temporanea e produzione, è necessario modificare le opzioni di pubblicazione del database. Non è più necessario eseguire gli script di concessione e distribuzione dei dati per il database delle appartenenze.

1. Aprire la procedura guidata **Pubblica sito Web** facendo clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliendo **pubblica**.
2. Fare clic sul profilo **test** nell'elenco a discesa **profilo** .
3. Fare clic sulla scheda **Impostazioni**.
4. In **DefaultConnection** nella sezione **database** deselezionare la casella di controllo **Aggiorna database** .
5. Fare clic sulla scheda **profilo** , quindi fare clic sul profilo di **gestione temporanea** nell'elenco a discesa **profilo** .
6. Quando viene chiesto se si desidera salvare le modifiche apportate al profilo di **test** , fare clic su **Sì**.
7. Apportare la stessa modifica nel profilo di gestione temporanea.
8. Ripetere il processo per apportare la stessa modifica nel profilo di produzione.
9. Chiudere la procedura guidata **Pubblica sito Web** .

La distribuzione nell'ambiente di test è ora una semplice operazione di esecuzione di un solo clic di pubblicazione. Per rendere più rapido questo processo, è possibile usare la barra degli strumenti **pubblica con un clic** .

1. Scegliere **barre degli strumenti** dal menu **Visualizza** e quindi **fare clic sul pulsante pubblica**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. In **Esplora soluzioni**selezionare il progetto ContosoUniversity.
3. sul **Web fare clic su pubblica** barra degli strumenti, scegliere il profilo di pubblicazione del **test** , quindi fare clic su **Pubblica Web** (icona con frecce che puntano a sinistra e a destra).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio distribuisce l'applicazione aggiornata e il browser si apre automaticamente al home page.
5. Eseguire la pagina insegnanti e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.

Normalmente si eseguono anche test di regressione, ovvero si testa il resto del sito per assicurarsi che la nuova modifica non interrompa le funzionalità esistenti. Tuttavia, per questa esercitazione si ignorerà questo passaggio e si procederà con la distribuzione dell'aggiornamento alla gestione temporanea e alla produzione.

Quando si esegue la ridistribuzione, Distribuzione Web determina automaticamente quali file sono stati modificati e copia solo i file modificati nel server. Per impostazione predefinita, Distribuzione Web usa le date delle ultime modifiche nei file per determinare quali sono state modificate. Alcuni sistemi di controllo del codice sorgente cambiano le date dei file anche se non si modifica il contenuto del file. In tal caso, potrebbe essere necessario configurare Distribuzione Web per utilizzare i checksum dei file per determinare quali file sono stati modificati. Per altre informazioni, vedere [perché tutti i file vengono ridistribuiti anche se non sono stati modificati?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nelle domande frequenti sulla distribuzione di ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Portare offline l'applicazione durante la distribuzione

La modifica che si sta distribuendo ora è una semplice modifica a una singola pagina. Tuttavia, a volte si distribuiscono modifiche di dimensioni maggiori o si distribuiscono modifiche al codice e al database e il sito potrebbe non funzionare correttamente se un utente richiede una pagina prima del completamento della distribuzione. Per impedire agli utenti di accedere al sito mentre è in corso la distribuzione, è possibile usare un' *app\_file offline. htm* . Quando si inserisce un file denominato *app\_offline. htm* nella cartella radice dell'applicazione, in IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, inserire l' *app\_offline. htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere l' *app\_offline. htm* dopo la corretta distribuzione.

È possibile configurare Distribuzione Web per inserire automaticamente un' *app predefinita\_file offline. htm* nel server al momento dell'avvio della distribuzione e della relativa rimozione al termine della distribuzione. A tale scopo, è necessario aggiungere l'elemento XML seguente al file del profilo di pubblicazione (con estensione pubxml):

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Per questa esercitazione si vedrà come creare e usare un' *app personalizzata\_file offline. htm* .

L'uso di *app\_offline. htm* nel sito di gestione temporanea non è obbligatorio perché non sono disponibili utenti che accedono al sito di gestione temporanea. Tuttavia, è consigliabile usare la gestione temporanea per testare tutte le modalità di distribuzione nell'ambiente di produzione.

### <a name="create-app_offlinehtm"></a>Crea app\_offline. htm

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
2. Creare una **pagina HTML** denominata *app\_offline. htm* (eliminare la "l" finale nell'estensione *HTML* creata da Visual Studio per impostazione predefinita).
3. Sostituire il markup del modello con il markup seguente:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salvare e chiudere il file.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Copiare l'app\_offline. htm nella cartella radice del sito Web

È possibile utilizzare qualsiasi strumento FTP per copiare i file nel sito Web. [FileZilla](http://filezilla-project.org/) è uno strumento FTP molto diffuso e viene visualizzato nelle schermate.

Per usare uno strumento FTP, sono necessari tre elementi: l'URL FTP, il nome utente e la password.

L'URL viene visualizzato nella pagina dashboard del sito Web nel portale di gestione di Azure e il nome utente e la password per FTP sono reperibili nel file con *estensione publishsettings* scaricato in precedenza. Nei passaggi seguenti viene illustrato come ottenere questi valori.

1. Nel portale di gestione di Azure fare clic sulla scheda **siti Web** , quindi fare clic sul sito Web di gestione temporanea.
2. Nella pagina **Dashboard** scorrere verso il basso fino a trovare il nome host FTP nella sezione **Quick Glance** .

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. Aprire il file staging *. publishsettings* nel blocco note o in un altro editor di testo.
4. Trovare l'elemento `publishProfile` per il profilo FTP.
5. Copiare i valori `userName` e `userPWD`.

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. Aprire lo strumento FTP e accedere all'URL FTP.
7. Copiare l' *app\_offline. htm* dalla cartella della soluzione alla cartella */site/wwwroot* nel sito di gestione temporanea.

    ![Copia app_offline](deploying-a-code-update/_static/image7.png)
8. Passare all'URL del sito di gestione temporanea. Si noterà che viene visualizzata la pagina *app\_offline. htm* invece della Home page.

    ![app_offline. htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

A questo punto si è pronti per la distribuzione in staging.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Distribuire l'aggiornamento del codice per la gestione temporanea e la produzione

1. Nella barra degli strumenti **pubblica sul Web fare clic** sul profilo di pubblicazione di **staging** , quindi scegliere **Pubblica sito Web**.

    Visual Studio distribuisce l'applicazione aggiornata e apre il browser all'home page del sito. Viene visualizzata l' *app\_file offline. htm* . Prima di poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere l' *app\_file offline. htm* .
2. Tornare allo strumento FTP ed eliminare l' **app\_offline. htm** dal sito di gestione temporanea.
3. Nel browser aprire la pagina insegnanti nel sito di gestione temporanea e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.
4. Seguire la stessa procedura per la produzione come per la gestione temporanea.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisione delle modifiche e distribuzione di file specifici

Visual Studio 2012 offre inoltre la possibilità di distribuire singoli file. Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione oppure copiare il file dall'ambiente di destinazione al progetto locale. In questa sezione dell'esercitazione si vedrà come usare queste funzionalità.

### <a name="make-a-change-to-deploy"></a>Apportare una modifica a deploy

1. Aprire *content/site. CSS*e trovare il blocco per il tag `body`.
2. Modificare il valore di `background-color` da `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Visualizzare la modifica nella finestra di anteprima di pubblicazione

Quando si utilizza la procedura guidata **Pubblica sito Web** per pubblicare il progetto, è possibile visualizzare le modifiche che verranno pubblicate facendo doppio clic sul file nella finestra di **Anteprima** .

1. Fare clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliere **pubblica**.
2. Dall'elenco a discesa **profilo** selezionare il profilo di pubblicazione del **test** .
3. Fare clic su **Anteprima**, quindi fare clic su **Avvia anteprima**.
4. Nel riquadro di **Anteprima** , fare doppio clic su **site. CSS**.

    ![Fare doppio clic su site. CSS](deploying-a-code-update/_static/image9.png)

    La finestra di dialogo **Anteprima modifiche** Mostra un'anteprima delle modifiche che verranno distribuite.

    ![Visualizzare in anteprima le modifiche apportate a site. CSS](deploying-a-code-update/_static/image10.png)

    Se si fa doppio clic sul file *Web. config* , la finestra di dialogo **Anteprima modifiche** Mostra l'effetto delle trasformazioni di configurazione della build e delle trasformazioni del profilo di pubblicazione. A questo punto non è stata eseguita alcuna operazione che comporterebbe la modifica del file *Web. config* nel server, pertanto si prevede di non visualizzare alcuna modifica. Tuttavia, la finestra **Anteprima modifiche** Mostra erroneamente due modifiche. Verranno rimossi due elementi XML. Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **esegui migrazioni Code First all'avvio dell'applicazione** per una classe del contesto di Code First. Il confronto viene eseguito prima che il processo di pubblicazione aggiunga tali elementi, quindi sembra che vengano rimossi anche se non verranno rimossi. Questo errore verrà corretto in una versione futura.
5. Fare clic su **Chiudi**.
6. Fare clic su **Pubblica**.
7. Quando si apre il browser alla home page del sito di test, premere CTRL + F5 per eseguire un aggiornamento hardware per vedere l'effetto della modifica CSS.

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. Chiudere il browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Pubblica file specifici da Esplora soluzioni

Si supponga di non avere lo sfondo blu e di voler ripristinare il colore originale. In questa sezione verranno ripristinate le impostazioni originali pubblicando un file specifico direttamente da **Esplora soluzioni**.

1. Aprire *content/site. CSS* e ripristinare l'impostazione di `background-color` su `#fff`.
2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul file *content/site. CSS* .

    Nel menu di scelta rapida vengono visualizzate tre opzioni di pubblicazione.

    ![Opzioni di pubblicazione da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. Fare clic su **Anteprima modifiche a site. CSS**.

    Viene visualizzata una finestra in cui sono illustrate le differenze tra il file locale e la versione dell'ambiente di destinazione.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. In **Esplora soluzioni**fare nuovamente clic con il pulsante destro del mouse su **site. CSS** e scegliere **Pubblica sito. CSS**.

    La finestra **attività di pubblicazione Web** indica che il file è stato pubblicato.

    ![Finestra attività di pubblicazione Web](deploying-a-code-update/_static/image14.png)
5. Aprire un browser all'URL `http://localhost/contosouniversity`, quindi premere CTRL + F5 per eseguire un aggiornamento rigido per visualizzare l'effetto della modifica CSS.

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. Chiudere il browser.

## <a name="summary"></a>Riepilogo

A questo punto sono stati illustrati diversi modi per distribuire un aggiornamento di un'applicazione che non implica una modifica al database e si è visto come visualizzare in anteprima le modifiche per verificare che ciò che verrà aggiornato è quello previsto. La pagina relativa agli insegnanti dispone ora di una sezione **corsi didattici** .

![Pagina relativa agli insegnanti con corsi insegnati](deploying-a-code-update/_static/image16.png)

Nell'esercitazione successiva viene illustrato come distribuire una modifica del database: verrà aggiunto un campo di data di nascita al database e alla pagina insegnanti.

> [!div class="step-by-step"]
> [Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)
