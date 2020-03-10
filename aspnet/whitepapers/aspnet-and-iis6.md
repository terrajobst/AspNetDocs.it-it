---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1,1 con IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Sebbene Windows Server 2003 includa sia IIS 6,0 che ASP.NET 1,1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper descrive come abilitare IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523310"
---
# <a name="running-aspnet-11-with-iis-60"></a>Esecuzione di ASP.NET 1.1 con IIS 6.0

> Sebbene Windows Server 2003 includa sia IIS 6,0 che ASP.NET 1,1, questi componenti sono disabilitati per impostazione predefinita. In questo white paper viene descritto come abilitare IIS 6,0 e ASP.NET 1,1 e vengono consigliate diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.
> 
> Si applica a ASP.NET 1,1 e IIS 6,0.

ASP.NET 1,1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6,0. IIS 6,0 e ASP.NET 1,1 sono progettati per integrarsi in modo semplice e ASP.NET ora il nuovo modello di processo di lavoro di IIS 6,0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1,1 non è installato per impostazione predefinita

Diversamente dalle versioni precedenti dei sistemi operativi server Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. e non è ASP.NET 1,1. Sono disponibili due opzioni per l'abilitazione di IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Abilitazione di IIS, opzione #1-Configurazione guidata server

Windows Server 2003 fornisce una nuova configurazione guidata server per semplificare la configurazione del server nella modalità desiderata.

Per avviare la procedura guidata, si noti che per eseguire la procedura guidata è necessario accedere come amministratore: andare a: Start | Programmi | Strumenti di amministrazione e selezionare "Configura server".

Una volta selezionata, verrà visualizzata la schermata di apertura della configurazione guidata server:

![](aspnet-and-iis6/_static/image1.jpg)

Fare clic su' avanti &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Fare clic su' avanti &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

In questa schermata sarà necessario selezionare "server applicazioni (IIS, ASP.NET)" come opzioni da configurare.

Fare clic su' avanti &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Dopo aver selezionato per configurare il server come server applicazioni, verrà visualizzata la schermata che richiede le funzionalità aggiuntive da installare. Per impostazione predefinita, nessuna delle due opzioni è selezionata. Per abilitare automaticamente ASP.NET, è necessario selezionare ' Abilita ASP. "NET".

Fare clic su' avanti &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

In questa schermata vengono visualizzate le opzioni da installare.

Fare clic su' avanti &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Questa schermata viene visualizzata durante l'installazione delle opzioni selezionate. È normale visualizzare le altre finestre di dialogo visualizzate durante l'installazione dei servizi. È inoltre possibile che venga richiesto il percorso del CD di installazione di Windows 2003 Server.

Al termine, fare clic su' avanti &gt;'.

![](aspnet-and-iis6/_static/image7.jpg)

Fare clic su' fine ': Windows Server 2003 è ora configurato per supportare IIS 6,0 e ASP.NET 1,1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Abilitazione di IIS, opzione #2-Configurazione manuale di IIS e ASP.NET

Se non si desidera utilizzare là Configurazione guidata server ', è possibile installare facoltativamente IIS 6,0 e ASP.NET 1,1 utilizzando ' installazione applicazioni ' dal pannello di controllo.

Aprire prima il pannello di controllo:

![](aspnet-and-iis6/_static/image8.jpg)

Quindi, fare clic su "Aggiungi/Rimuovi componenti di Windows" per aprire la "procedura guidata componenti di Windows":

![](aspnet-and-iis6/_static/image9.jpg)

Evidenziare e selezionare "server applicazioni", quindi fare clic su "dettagli". pulsante

![](aspnet-and-iis6/_static/image10.jpg)

Per installare ASP.NET, selezionare ' ASP. "NET".

Fare clic su' OK ' per tornare alla procedura guidata componenti di Windows. Fare clic su' avanti &gt;' dalla procedura guidata componenti di Windows per avviare l'installazione:

![](aspnet-and-iis6/_static/image11.jpg)

È normale visualizzare le altre finestre di dialogo visualizzate durante l'installazione dei servizi. È inoltre possibile che venga richiesto il percorso del CD di installazione di Windows 2003 Server.

Al termine dell'installazione, verrà visualizzata l'ultima schermata della creazione guidata componente di Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6,0 e ASP.NET 1,1 sono ora configurati e disponibili.

## <a name="recommended-settings"></a>Impostazioni consigliate

Quando si esegue ASP.NET 1,1 con IIS 6,0 sono disponibili diverse impostazioni di configurazione consigliate per ottenere prestazioni ottimali da ASP.NET:

- Configurazione dei limiti di memoria del processo di lavoro
- Configurazione del riciclo del processo di lavoro

### <a name="configuring-worker-process-memory-limits"></a>Configurazione dei limiti di memoria del processo di lavoro

Per impostazione predefinita, IIS 6,0 non imposta un limite per la quantità di memoria che IIS può utilizzare. ASP. La funzionalità cache di NET si basa su una limitazione della memoria in modo che la cache possa rimuovere in modo proattivo gli elementi inutilizzati dalla memoria.

Si consiglia di configurare la funzionalità di riciclo della memoria di IIS 6,0. Per configurare questo gestore di Internet Information Services aperto (Start | Programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperta, espandere la cartella ' pool di applicazioni ':

Per ogni pool di applicazioni:

![](aspnet-and-iis6/_static/image13.jpg)

1. Fare clic con il pulsante destro del mouse sul pool di applicazioni, ad esempio ' DefaultAppPool ', quindi selezionare ' proprietà':

![](aspnet-and-iis6/_static/image14.jpg)

2. Abilitare quindi il riciclo della memoria facendo clic su' memoria massima usata (in megabyte):'. Il valore non deve essere maggiore della quantità di memoria fisica (non virtuale) nel server, una corretta approssimazione è il 60% della memoria fisica, ovvero per un server con 512 MB di memoria fisica selezionare 310. È inoltre consigliabile che il valore massimo non superi 800MB quando si utilizza uno spazio degli indirizzi da 2 GB. Se lo spazio degli indirizzi di memoria del server è 3GB, il limite massimo di memoria per il processo di lavoro può essere pari a 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Fare clic su "applica" e su "OK" per uscire dalla finestra di dialogo delle proprietà. Ripetere questa operazione per tutti i pool di applicazioni disponibili.

### <a name="configuring-worker-recycling"></a>Configurazione del riciclo del ruolo di lavoro

Per impostazione predefinita, IIS 6,0 è configurato per riciclare il processo di lavoro ogni 29 ore. Si tratta di un po' aggressivo per un'applicazione che esegue ASP.NET ed è consigliabile disabilitare il riciclo automatico dei processi di lavoro.

Per disabilitare il riciclo automatico del processo di lavoro, aprire prima Internet Information Services Manager (Start | Programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperta, espandere la cartella ' pool di applicazioni ':

![](aspnet-and-iis6/_static/image16.jpg)

Per ogni pool di applicazioni:

1. Fare clic con il pulsante destro del mouse sul pool di applicazioni, ad esempio ' DefaultAppPool ', quindi selezionare ' proprietà':

![](aspnet-and-iis6/_static/image17.jpg)

2. Deseleziona ' riciclo processo di lavoro (in minuti):':

![](aspnet-and-iis6/_static/image18.jpg)

Fare clic su "applica" e su "OK" per uscire dalla finestra di dialogo delle proprietà. Ripetere questa operazione per tutti i pool di applicazioni disponibili.

## <a name="granting-write-access-to-the-file-system"></a>Concessione dell'accesso in scrittura al file system

Se l'applicazione richiede l'accesso in scrittura al file system e si utilizza NTFS, sarà necessario modificare un elenco di controllo di accesso (ACL) nella cartella o nel file per concedere l'accesso ASP.NET a.

Ad esempio, per concedere a ASP.NET l'accesso in scrittura alla c:\Inetpub\Wwwroot prima apertura di Esplora risorse e passare alla directory:

![](aspnet-and-iis6/_static/image19.jpg)

Fare quindi clic con il pulsante destro del mouse sulla directory, ad esempio "wwwroot" e selezionare Proprietà. Quando viene visualizzata la finestra di dialogo Proprietà, selezionare la scheda "sicurezza":

![](aspnet-and-iis6/_static/image20.jpg)

La directory c:\inetpub\wwwroot\ è una directory speciale in cui lo speciale gruppo IIS 6,0' IIS\_WPG ' ha già concesso la lettura &amp; eseguire, elencare il contenuto della cartella e le autorizzazioni di lettura. Tuttavia, per concedere l'autorizzazione di scrittura, è necessario fare clic sulla casella di controllo Consenti per la scrittura:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6,0 ora dispone dell'autorizzazione di scrittura per questa cartella. Per concedere autorizzazioni di scrittura per altre cartelle, attenersi alla seguente procedura: si noti che potrebbe essere necessario aggiungere il gruppo di IIS\_WPG se non esiste già.

> [!CAUTION]
> La concessione dell'autorizzazione di scrittura per IIS\_WPG consentirà a qualsiasi applicazione ASP.NET di scrivere in questa directory.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Supporto dell'autenticazione integrata con SQL Server

L'autenticazione integrata consente SQL Server di utilizzare l'autenticazione di Windows NT per convalidare gli account di accesso SQL Server. Questo consente all'utente di ignorare il processo di accesso SQL Server standard. Con questo approccio, un utente di rete può accedere a un database di SQL Server senza specificare una password o un identificatore di accesso separato perché SQL Server Ottiene le informazioni sull'utente e sulla password dal processo di sicurezza di rete di Windows NT.

La scelta dell'autenticazione integrata per le applicazioni ASP.NET è una scelta ottimale perché non vengono mai archiviate credenziali nella stringa di connessione per l'applicazione. Piuttosto la stringa di connessione usata per connettersi a SQL sarà simile alla seguente:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Questa stringa di connessione indica SQL Server di utilizzare le credenziali di Windows dell'applicazione che tenta di accedere a SQL Server. Nel caso di ASP.NET/IIS 6, si tratta di un account nel gruppo di IIS\_WPG.

Per abilitare l'autenticazione integrata tra SQL Server e ASP.NET, è necessario prima di tutto assicurarsi che SQL Server sia configurato per l'autenticazione integrata o per l'autenticazione in modalità mista. verificare con l'amministratore di database per determinare questo problema. Se SQL Server è in una di queste due modalità, è possibile usare l'autenticazione integrata.

Aprire SQL Server Enterprise Manager (Start | Programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:

![](aspnet-and-iis6/_static/image22.jpg)

Se il gruppo ' BUILTINT\IIS\_WPG ' non è elencato, fare clic con il pulsante destro del mouse su account di accesso e selezionare ' nuovo login:

![](aspnet-and-iis6/_static/image23.jpg)

Nella casella di testo "Name:" immettere "[Server/nome dominio] \IIS\_WPG" oppure fare clic sul pulsante con i puntini di sospensione per aprire il selettore utenti/gruppi di Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Selezionare il gruppo di IIS\_WPG del computer corrente, fare clic su' Aggiungi ' e OK per chiudere la selezione.

Sarà quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database. Per impostare il database predefinito, scegliere dall'elenco a discesa, ad esempio, di seguito è selezionato Northwind:

![](aspnet-and-iis6/_static/image25.jpg)

Fare quindi clic sulla scheda accesso al database:

![](aspnet-and-iis6/_static/image26.jpg)

Fare clic sulla casella di controllo Consenti per ogni database a cui si desidera consentire l'accesso. È inoltre necessario selezionare i ruoli del database, controllando il proprietario del database\_assicurerà che l'account di accesso disponga di tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.

Fare clic su OK per uscire dalla finestra di dialogo delle proprietà. L'applicazione ASP.NET è ora configurata per supportare l'autenticazione integrata del SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Non eseguire ASP.NET 1,0 in modalità nativa di IIS 6,0

ASP.NET 1,0 in IIS 6,0 è supportato solo in modalità di compatibilità con IIS 5.

Per configurare ASP.NET 1,0 per l'esecuzione in modalità di compatibilità IIS 5,0, aprire Gestione servizi Internet e fare clic con il pulsante destro del mouse su siti Web e scegliere Proprietà:

![](aspnet-and-iis6/_static/image27.jpg)

Passare alla scheda servizio e selezionare? Eseguire il servizio WWW in modalità di isolamento IIS 5,0?:

![](aspnet-and-iis6/_static/image28.jpg)
