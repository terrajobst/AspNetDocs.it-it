---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: distribuzione di un database SQL Server aggiornamento-11 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621062"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento del database SQL Server-11 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in siti Web di Microsoft Azure, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come distribuire un aggiornamento del database in un database di SQL Server completo. Poiché Migrazioni Code First esegue tutte le operazioni di aggiornamento del database, il processo è quasi identico a quello che è stato fatto per SQL Server Compact nell'esercitazione [distribuzione di un aggiornamento del database](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione dell'esercitazione verrà apportata una modifica al database e le modifiche al codice corrispondenti, quindi verranno testate in Visual Studio per prepararle per la distribuzione negli ambienti di test e di produzione. La modifica comporta l'aggiunta di una colonna `OfficeHours` all'entità `Instructor` e la visualizzazione delle nuove informazioni nella pagina Web **Instructors** .

Nel progetto ContosoUniversity. DAL aprire *Instructor.cs* e aggiungere la proprietà seguente tra le proprietà `HireDate` e `Courses`:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aggiornare la classe inizializzatore in modo che seedi la nuova colonna con i dati di test. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con il blocco di codice seguente, che include la nuova colonna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity aprire *Instructors. aspx* e aggiungere un nuovo campo modello per le ore di ufficio immediatamente prima del tag di chiusura `</Columns>` nel primo controllo `GridView`:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compila la soluzione.

Aprire la finestra **console di gestione pacchetti** e selezionare CONTOSOUNIVERSITY. dal come **progetto predefinito**.

Immettere i comandi seguenti:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Eseguire l'applicazione e selezionare la pagina **Instructors (insegnanti** ). Il caricamento della pagina richiede un po' più tempo del solito, perché il Entity Framework ricrea il database e lo Seed con i dati di test.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Distribuzione dell'aggiornamento del database nell'ambiente di test

Quando si utilizza Migrazioni Code First, il metodo per la distribuzione di una modifica del database in SQL Server è identico a quello di SQL Server Compact. Tuttavia, è necessario modificare il profilo di pubblicazione del test perché è ancora configurato per eseguire la migrazione da SQL Server Compact a SQL Server.

Il primo passaggio consiste nel rimuovere le trasformazioni delle stringhe di connessione create nell'esercitazione precedente. Queste non sono più necessarie perché si specificheranno le trasformazioni delle stringhe di connessione nel profilo di pubblicazione, come è stato fatto prima della configurazione della scheda **pubblicazione/pubblicazione pacchetto SQL** per la migrazione a SQL Server.

Aprire il file *Web. test. config* e rimuovere l'elemento `connectionStrings`. L'unica trasformazione rimanente nel file *Web. test. config* è per il valore `Environment` nell'elemento `appSettings`.

A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicarlo nell'ambiente di test.

Aprire la procedura guidata **Pubblica sito Web** , quindi passare alla scheda **profilo** .

Selezionare il profilo di pubblicazione del **test** .

Selezionare la scheda **Impostazioni**.

Fare clic su **Abilita miglioramenti alla pubblicazione del nuovo database**.

Nella casella stringa di connessione per **schoolContext**immettere lo stesso valore usato nel file di trasformazione *Web. test. config* nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selezionare **esegui migrazioni Code First (eseguito all'avvio dell'applicazione)** . Nella versione di Visual Studio, la casella di controllo potrebbe essere denominata **Apply migrazioni Code First**.

Nella casella stringa di connessione per **DefaultConnection**immettere lo stesso valore usato nel file di trasformazione *Web. test. config* nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lasciare deselezionata l' **aggiornamento database** .

Fare clic su **Pubblica**.

Visual Studio distribuisce le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.

Selezionare la pagina Instructors (insegnanti).

Quando l'applicazione esegue questa pagina, tenta di accedere al database. Migrazioni Code First controlla se il database è aggiornato e rileva che l'ultima migrazione non è ancora stata applicata. Migrazioni Code First applica la migrazione più recente, esegue il metodo `Seed`, quindi la pagina viene eseguita normalmente. Viene visualizzata la nuova colonna ore di ufficio con i dati di seeding.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Distribuzione dell'aggiornamento del database nell'ambiente di produzione

È necessario modificare anche il profilo di pubblicazione per l'ambiente di produzione. In questo caso verrà rimosso il profilo esistente e ne verrà creato uno nuovo importando un file con estensione publishsettings aggiornato. Il file aggiornato includerà la stringa di connessione per il database di SQL Server in Cytanium.

Come si è visto quando è stata eseguita la distribuzione nell'ambiente di test, non sono più necessarie trasformazioni della stringa di connessione nel file di trasformazione *Web. Production. config* . Aprire il file e rimuovere l'elemento `connectionStrings`. Le trasformazioni rimanenti sono relative al valore `Environment` nell'elemento `appSettings` e all'elemento `location` che limita l'accesso ai report degli errori ELMAH.

Prima di creare un nuovo profilo di pubblicazione per la produzione, scaricare un file con estensione publishsettings aggiornato nello stesso modo in cui si è fatto in precedenza nell'esercitazione [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Nel pannello di controllo di Cytanium fare clic su **siti Web**, quindi fare clic sul sito Web **contosouniversity.com** . Selezionare la scheda **pubblicazione sul Web** , quindi fare clic su **Scarica profilo di pubblicazione per il sito Web**. Il motivo per cui si esegue questa operazione consiste nel prelevare la stringa di connessione del database nel file con estensione publishsettings. La stringa di connessione non era disponibile la prima volta che è stato scaricato il file, perché si stava ancora usando SQL Server Compact e non era ancora stato creato il database di SQL Server in Cytanium.

A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicarlo nell'ambiente di produzione.

Aprire la procedura guidata **Pubblica sito Web** , quindi passare alla scheda **profilo** .

Fare clic su **Gestisci profili**e quindi eliminare il profilo di produzione.

Chiudere la procedura guidata **Pubblica sito Web** per salvare la modifica.

Aprire di nuovo la procedura guidata **Pubblica sito Web** , quindi fare clic su **Importa**.

Nella scheda **connessione** modificare l' **URL di destinazione** con il valore appropriato se si utilizza un URL temporaneo.

Scegliere **Avanti**.

Nella scheda **Impostazioni** fare clic su **Abilita nuovi miglioramenti alla pubblicazione del database**.

Nell'elenco a discesa stringa di connessione per **schoolContext**selezionare la stringa di connessione Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selezionare **Execute Code First Migrations (eseguito all'avvio dell'applicazione)** .

Nell'elenco a discesa stringa di connessione per **DefaultConnection**selezionare la stringa di connessione Cytanium.

Selezionare la scheda **profilo** , fare clic su **Gestisci profili**e rinominare il profilo da "contosouniversity.com-distribuzione Web" a "produzione".

Chiudere il profilo di pubblicazione per salvare la modifica, quindi aprirla di nuovo.

Fare clic su **Pubblica**. Per un sito Web di produzione reale, è necessario copiare l' *app\_offline. htm* in produzione e inserirla nella cartella del progetto prima della pubblicazione, quindi rimuoverla al termine della distribuzione.

Visual Studio distribuisce le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.

Selezionare la pagina Instructors (insegnanti).

Migrazioni Code First aggiorna il database nello stesso modo in cui è stato fatto nell'ambiente di test. Viene visualizzata la nuova colonna ore di ufficio con i dati di seeding.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

A questo punto è stato distribuito un aggiornamento dell'applicazione che includeva una modifica al database usando un database SQL Server.

## <a name="more-information"></a>Altre informazioni

Questa operazione completa questa serie di esercitazioni sulla distribuzione di un'applicazione Web ASP.NET a un provider di hosting di terze parti. Per ulteriori informazioni sugli argomenti trattati in queste esercitazioni, vedere la pagina relativa alla [mappa del contenuto della distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) sul sito Web MSDN.

## <a name="acknowledgements"></a>Riconoscimenti

Desidero ringraziare i seguenti utenti che hanno apportato contributi significativi al contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP per lo sviluppo di piattaforme dati, Stati Uniti
- Duro Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Sauro Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott hanseln](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- Alessandro [Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
