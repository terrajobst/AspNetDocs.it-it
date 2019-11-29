---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636824"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Distribuzione Web ASP.NET con Visual Studio: distribuzione di un aggiornamento del database

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica di

In questa esercitazione si apportano modifiche al database e modifiche del codice correlate, si verificano le modifiche in Visual Studio, quindi si distribuisce l'aggiornamento negli ambienti di test, di gestione temporanea e di produzione.

Nell'esercitazione viene innanzitutto illustrato come aggiornare un database gestito da Migrazioni Code First e successivamente viene illustrato come aggiornare un database utilizzando il provider dbDacFx.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Distribuire un aggiornamento del database usando Migrazioni Code First

In questa sezione viene aggiunta una colonna della data di nascita alla classe di base `Person` per le entità `Student` e `Instructor`. Si aggiorna quindi la pagina in cui vengono visualizzati i dati dell'insegnante, in modo che venga visualizzata la nuova colonna. Infine, vengono distribuite le modifiche apportate a test, gestione temporanea e produzione.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Aggiungere una colonna a una tabella nel database dell'applicazione

1. Nel progetto *ContosoUniversity. dal* aprire *Person.cs* e aggiungere la proprietà seguente alla fine della classe `Person` (sono presenti due parentesi graffe di chiusura successive):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Successivamente, aggiornare il metodo `Seed` in modo che fornisca un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con il blocco di codice seguente, che include informazioni sulla data di nascita:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** . Assicurarsi che ContosoUniversity. DAL sia ancora selezionato come **progetto predefinito**.
3. Nella finestra **console di gestione pacchetti** selezionare **ContosoUniversity. dal** come **progetto predefinito**, quindi immettere il comando seguente:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe `DbMigration` e nel metodo `Up` è possibile visualizzare il codice che crea la nuova colonna. Il metodo `Up` crea la colonna quando si implementa la modifica e il metodo `Down` Elimina la colonna quando si esegue il rollback della modifica.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compilare la soluzione, quindi immettere il comando seguente nella finestra della **console di gestione pacchetti** (assicurarsi che il progetto CONTOSOUNIVERSITY. dal sia ancora selezionato):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Il Entity Framework esegue il metodo `Up` e quindi esegue il `Seed` metodo.

### <a name="display-the-new-column-in-the-instructors-page"></a>Visualizza la nuova colonna nella pagina insegnanti

1. Nel progetto ContosoUniversity aprire *Instructors. aspx* e aggiungere un nuovo campo modello per visualizzare la data di nascita. Aggiungerlo tra quelli per data di assunzione e assegnazione dell'ufficio:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    Se il rientro del codice non viene sincronizzato, è possibile premere CTRL + K, quindi CTRL + D per riformattare automaticamente il file.
2. Eseguire l'applicazione e fare clic sul collegamento **Instructors (insegnanti** ).

    Quando la pagina viene caricata, si noterà che contiene il nuovo campo Data di nascita.

    ![Pagina degli insegnanti con la nascita](deploying-a-database-update/_static/image2.png)
3. Chiudere il browser.

### <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

1. In **Esplora soluzioni** selezionare il progetto ContosoUniversity.
2. Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione del **test** , quindi fare clic su **Pubblica sito Web**. Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity in **Esplora soluzioni**.

    Visual Studio distribuisce l'applicazione aggiornata e il browser si apre al home page.
3. Eseguire la pagina **insegnanti** per verificare che l'aggiornamento sia stato distribuito correttamente.

    Quando l'applicazione tenta di accedere al database per questa pagina, Code First aggiorna lo schema del database ed esegue il `Seed` metodo. Quando viene visualizzata la pagina, viene visualizzata la colonna della **Data di nascita** prevista con le date al suo interno.
4. Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione di **staging** , quindi fare clic su **Pubblica sito Web**.
5. Eseguire la pagina **insegnanti** in staging per verificare che l'aggiornamento sia stato distribuito correttamente.
6. Nella barra degli strumenti **pubblica sul Web fare** clic sul profilo di pubblicazione di **produzione** , quindi fare clic su **Pubblica sito Web**.
7. Eseguire la pagina **insegnanti** in produzione per verificare che l'aggiornamento sia stato distribuito correttamente.

    Per un aggiornamento di un'applicazione di produzione reale che include una modifica del database, l'applicazione viene in genere disconnessa durante la distribuzione usando l' *app\_offline. htm*, come illustrato nell'esercitazione precedente.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Distribuire un aggiornamento del database usando il provider dbDacFx

In questa sezione si aggiunge una colonna *Commenti* alla tabella *utente* nel database di appartenenze e si crea una pagina che consente di visualizzare e modificare i commenti per ogni utente. Quindi si distribuiscono le modifiche in test, gestione temporanea e produzione.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Aggiungere una colonna a una tabella nel database delle appartenenze

1. In Visual Studio aprire **Esplora oggetti di SQL Server**.
2. Espandere **(local DB) \v11.0**, espandere **database**, **ASPNET-ContosoUniversity** (non **ASPNET-ContosoUniversity-prod**), quindi espandere **tabelle**.

    Se non viene visualizzato **(local DB) \v11.0** nel nodo **SQL Server** , fare clic con il pulsante destro del mouse sul nodo **SQL Server** e scegliere **Aggiungi SQL Server**. Nella finestra di dialogo **Connetti al server** immettere *(local DB) \v11.0* come **nome del server**, quindi fare clic su **Connetti**.

    Se **ASPNET-ContosoUniversity**non viene visualizzato, eseguire il progetto e accedere usando le credenziali di *amministratore* (password è *devpwd*), quindi aggiornare la finestra **Esplora oggetti di SQL Server** .
3. Fare clic con il pulsante destro del mouse sulla tabella **Users** , quindi scegliere **Visualizza finestra di progettazione**.

    ![Progettazione viste SSOX](deploying-a-database-update/_static/image3.png)
4. Nella finestra di progettazione aggiungere una colonna *Commenti* e renderla *nvarchar (128)* e Nullable, quindi fare clic su **Aggiorna**.

    ![Aggiunta della colonna Commenti](deploying-a-database-update/_static/image4.png)
5. Nella casella **Anteprima database aggiornamenti** , fare clic su **Aggiorna database**.

    ![Anteprima aggiornamenti database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Creare una pagina per visualizzare e modificare la nuova colonna

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella **account** nel progetto ContosoUniversity, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
2. Creare un nuovo **Web form usando la pagina master** e denominarlo *UserInfo. aspx*. Accettare il file *site. master* predefinito come pagina master.
3. Copiare il markup seguente nell'elemento `MainContent` `Content` (l'ultimo dei tre elementi `Content`):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Fare clic con il pulsante destro del mouse sulla pagina *UserInfo. aspx* e scegliere **Visualizza nel browser**.
5. Accedere con le credenziali utente *amministratore* (la password è *devpwd*) e aggiungere alcuni commenti a un utente per verificare che la pagina funzioni correttamente.

    ![Pagina UserInfo](deploying-a-database-update/_static/image6.png)
6. Chiudere il browser.

## <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

Per eseguire la distribuzione usando il provider dbDacFx, è sufficiente selezionare l'opzione **Aggiorna database** nel profilo di pubblicazione. Tuttavia, per la distribuzione iniziale quando è stata usata questa opzione, sono stati configurati anche alcuni script SQL aggiuntivi da eseguire: quelli ancora presenti nel profilo ed è necessario impedirne l'esecuzione.

1. Aprire la procedura guidata **Pubblica sito Web** facendo clic con il pulsante destro del mouse sul progetto ContosoUniversity e scegliendo **pubblica**.
2. Selezionare il profilo di **test** .
3. Fare clic sulla scheda **Impostazioni** .
4. In **DefaultConnection**selezionare **Aggiorna database**.
5. Disabilitare gli script aggiuntivi configurati per l'esecuzione per la distribuzione iniziale:

    1. Fare clic su **Configura aggiornamenti database**.
    2. Nella finestra di dialogo **Configura aggiornamenti database** deselezionare le caselle di controllo accanto a *concedi. SQL* e *ASPNET-data-dev. SQL*.
    3. Fare clic su **Chiudi**.
6. Fare clic sulla scheda **Anteprima** .
7. In **database** e a destra di **DefaultConnection**fare clic sul collegamento **Anteprima database** .

    ![Anteprima database](deploying-a-database-update/_static/image7.png)

    Nella finestra di anteprima viene visualizzato lo script che verrà eseguito nel database di destinazione per fare in modo che lo schema del database corrisponda allo schema del database di origine. Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.
8. Chiudere la finestra di dialogo **Anteprima database** , quindi fare clic su **pubblica**.

    Visual Studio distribuisce l'applicazione aggiornata e il browser si apre al home page.
9. Eseguire la pagina UserInfo (aggiungere *account/UserInfo. aspx* all'URL Home Page) per verificare che l'aggiornamento sia stato distribuito correttamente. Dovrai accedere immettendo *admin* e *devpwd*.

    I dati nelle tabelle non vengono distribuiti per impostazione predefinita e non è stato configurato uno script di distribuzione dei dati per l'esecuzione, quindi non sarà possibile trovare il commento aggiunto in fase di sviluppo. È possibile aggiungere ora un nuovo commento in gestione temporanea per verificare che la modifica sia stata distribuita nel database e che la pagina funzioni correttamente.
10. Seguire la stessa procedura per eseguire la distribuzione in gestione temporanea e produzione.

    Non dimenticare di disabilitare gli script aggiuntivi. L'unica differenza rispetto al profilo di test è che verrà disabilitato solo uno script nei profili di gestione temporanea e di produzione perché sono stati configurati per l'esecuzione solo di *ASPNET-prod-Data. SQL*.

    Le credenziali per la gestione temporanea e la produzione sono admin e prodpwd.

    Per un aggiornamento di un'applicazione di produzione reale che include una modifica del database, in genere l'applicazione viene disconnessa durante la distribuzione caricando l' *app\_offline. htm* prima di pubblicarla ed eliminarla successivamente, come illustrato nell' [esercitazione precedente](deploying-a-code-update.md).

## <a name="summary"></a>Riepilogo

A questo punto è stato distribuito un aggiornamento dell'applicazione che includeva una modifica del database usando sia Migrazioni Code First che il provider dbDacFx.

![Pagina degli insegnanti con la nascita](deploying-a-database-update/_static/image8.png)

![Pagina UserInfo](deploying-a-database-update/_static/image9.png)

L'esercitazione successiva illustra come eseguire le distribuzioni usando la riga di comando.

> [!div class="step-by-step"]
> [Precedente](deploying-a-code-update.md)
> [Successivo](command-line-deployment.md)
