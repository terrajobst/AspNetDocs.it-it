---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Miglioramenti in Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575579"
---
# <a name="improvements-in-visual-studio-2005"></a>Miglioramenti in Visual Studio 2005

[Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web.

Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web un lungo elenco di miglioramenti e miglioramenti per i progetti Web. Quanto più potente di Visual Studio .NET 2002 e 2003, si sono verificati molti problemi nel modo in cui i progetti web sono stati gestiti. Visual Studio 2005 aggiunge un numero significativo di nuove funzionalità per risolvere questi reclami. Per coloro che preferiscono il modo in cui Visual Studio .NET 2003 gestisce la compilazione di applicazioni Web, vedere [progetti di applicazione Web](https://go.microsoft.com/fwlink/?LinkId=57870).

In questo modulo sono stati apportati miglioramenti per la creazione, la gestione e lo sviluppo di progetti Web. In un modulo successivo, vengono descritti i miglioramenti apportati alla compilazione di progetti Web e alla loro distribuzione.

## <a name="frontpage-server-extensions"></a>Estensioni del server di FrontPage

Visual Studio .NET 2002 e 2003 sono necessari Estensioni del server di FrontPage nella casella per creare o compilare progetti Web. Gli sviluppatori hanno scelto tra due diverse modalità di accesso (Estensioni del server di FrontPage o modalità di accesso ai file), entrambi usati Estensioni del server di FrontPage per eseguire attività quali l'impostazione della radice dell'applicazione in IIS e così via.

Visual Studio 2005 rimuove la dipendenza da Estensioni del server di FrontPage per i progetti locali. Visual Studio 2005 ora accede direttamente alla metabase IIS invece di usare la Estensioni del server di FrontPage. Visual Studio 2005 aggiunge anche il supporto per FTP, che consente l'accesso al progetto remoto senza richiedere Estensioni del server di FrontPage.

Per gli sviluppatori che desiderano utilizzare Estensioni del server di FrontPage nei propri progetti, l'opzione è ancora disponibile. Tuttavia, in base al forte feedback della community degli sviluppatori ASP.NET, non è un requisito.

> [!NOTE]
> Estensioni del server di FrontPage sono ancora necessari per la creazione, l'apertura e così via del progetto remoto.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Visual Studio 2005 viene fornito con un nuovo server Web denominato Server di sviluppo ASP.NET. (Questo server Web era precedentemente noto come Cassini).

Il Server di sviluppo ASP.NET presenta diversi vantaggi.

- È ora possibile per gli amministratori non di sviluppare ed eseguire il debug in un server Web.
- Il Server di sviluppo ASP.NET esegue dinamicamente il mapping delle directory virtuali a qualsiasi posizione nel file system consentendo percorsi flessibili dei progetti.
- Gli utenti di Windows XP Professional che usano già IIS saranno ora in grado di creare nuove applicazioni Web che non influiscono sulla struttura di file o cartelle del sito Web predefinito in IIS.

Non è necessaria alcuna configurazione speciale per sfruttare i vantaggi del Server di sviluppo ASP.NET. Quando viene eseguito il debug o l'esplorazione di un progetto Web ospitato nella file system, Visual Studio 2005 avvierà automaticamente un'istanza del Server di sviluppo ASP.NET su una porta casuale per servire la richiesta.

Ulteriori informazioni verranno analizzate nella Server di sviluppo ASP.NET più avanti in questo modulo.

## <a name="improved-file-management"></a>Gestione migliorata dei file

In Visual Studio 2002 e 2003, un file di progetto (. vbproj per VB.NET e. csproj C#per) ha archiviato le informazioni su tutti i file nell'applicazione Web. La visualizzazione Esplora soluzioni si basa sulle informazioni del file nel file di progetto. Per questo motivo, le Esplora soluzioni spesso visualizzano informazioni non accurate nei casi in cui sono stati utilizzati editor esterni. Visual Studio 2002 e 2003 spesso sovrascrivono le modifiche ai file o non visualizzano la versione più recente dei file.

Visual Studio 2005 esegue questa operazione con il file di progetto. Ma legge le informazioni su file e cartelle direttamente dal disco, ottenendo una visualizzazione accurata dei file nel progetto. Poiché la cartella References in Visual Studio 2002 e 2003 non rappresenta una cartella effettiva nell'applicazione Web, Visual Studio 2005 rimuove anche la cartella References da Esplora soluzioni. Per accedere ai riferimenti per il progetto in Visual Studio 2005, è necessario usare le pagine delle proprietà per il progetto.

## <a name="creating-web-projects"></a>Creazione di progetti Web

Gli sviluppatori Web hanno molte nuove opzioni disponibili per la creazione di progetti in Visual Studio 2005. È ora possibile creare siti Web in qualsiasi punto del file system e quindi eseguirne il debug o esplorarli usando la nuova Server di sviluppo ASP.NET. Gli sviluppatori possono inoltre creare nuovi siti Web utilizzando FTP.

Fare clic qui per visualizzare una procedura dettagliata video per la creazione di progetti Web in Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Progetti file System

Come si è visto nella procedura dettagliata del video, è possibile scegliere di creare siti Web nel file system nel computer locale o in una posizione remota tramite una condivisione file. I siti Web creati nel file system vengono visualizzati e sottoposti a debug utilizzando l'Server di sviluppo ASP.NET.

> [!NOTE]
> Il Server di sviluppo ASP.NET può causare confusione per i clienti. Se viene creato un progetto Web nella file system nella struttura di directory IISs (ad esempio, c:/Inetpub/wwwroot), il sito Web verrà comunque esplorato tramite l'Server di sviluppo ASP.NET quando viene avviato dall'interno di Visual Studio 2005. Pertanto, qualsiasi configurazione di IIS (ovvero metodi di autenticazione) non è applicabile.

Il progetto Web predefinito rimuove anche una grande quantità di overhead includendo solo una pagina default. aspx, un file default.cs e una cartella app/_Data. Il file Web. config e le cartelle speciali (ad esempio app/_code) vengono aggiunti quando sono necessari. Il progetto Web include solo i file e le cartelle necessari.

### <a name="http-projects"></a>Progetti HTTP

I progetti HTTP possono essere progetti creati in un sito Web IIS locale o in un sito Web remoto. Il percorso predefinito del progetto è `http://localhost`. Se si fa clic sul pulsante Sfoglia, sono disponibili due opzioni HTTP: IIS locale e sito remoto. La differenza principale tra queste due opzioni è il metodo in cui le informazioni sul sito Web vengono visualizzate nella finestra di dialogo Scegli percorso e la modalità di copia dei file nel server Web.

L'opzione IIS locale legge le informazioni del sito dalla metabase nel computer locale e i file vengono copiati usando il file system. L'opzione sito remoto usa il Estensioni del server di FrontPage e le informazioni e i file del sito vengono copiati usando le chiamate RPC HTTP e Estensioni del server di FrontPage.

> [!NOTE]
> Il file vs # # #/_tmp. htm e Get/_aspx/_ver. aspx non vengono più utilizzati per determinare le informazioni sulla versione.

L'opzione HTTP predefinita è IIS locale. Questa opzione consente di leggere la metabase IIS per determinare quali siti sono disponibili e il percorso in cui creare il contenuto. È possibile selezionare una cartella o una directory virtuale diversa selezionandola nella visualizzazione albero. È inoltre possibile creare una nuova directory virtuale, contrassegnare le cartelle come applicazioni, nonché eliminare le directory virtuali esistenti da questa finestra di dialogo.

![Finestra di dialogo Scegli percorso](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: finestra di dialogo Scegli percorso

Diversamente dalle versioni precedenti di Visual Studio, se si seleziona la casella di controllo **usa Secure Sockets Layer** e il certificato SSL non corrisponde all'URL che si sta esplorando, viene visualizzata una finestra di dialogo di avviso di sicurezza in cui viene chiesto se si desidera procedere. Se si usa Visual Studio .NET 2003, se il certificato non corrisponde, la creazione del progetto avrà esito negativo.

![Avviso di sicurezza relativo al certificato SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: avviso di sicurezza relativo al certificato SSL

### <a name="note-on-host-headers"></a>Nota sulle intestazioni host

Se si sta creando un'applicazione Web in un sito associato a un indirizzo IP specifico, sarà necessario assicurarsi che sia configurata un'intestazione host. In caso contrario, Visual Studio creerà il sito in `http://localhost`, ma l'indirizzo IP non verrà risolto correttamente quando il sito viene esplorato o sottoposto a debug dall'interno dell'IDE.

Se si seleziona l'opzione sito remoto, la finestra di dialogo Cambia per consentire all'utente di immettere l'URL di destinazione per il nuovo sito Web. Questo URL deve trovarsi in un server in cui è abilitata la Estensioni del server di FrontPage. Se si desidera lavorare con il server Web locale utilizzando il Estensioni del server di FrontPage, è possibile utilizzare l'opzione sito remoto e specificare un URL locale.

![Creazione di un sito Web su un server remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: creazione di un sito Web su un server remoto

Quando si crea un'applicazione in un sito remoto tramite SSL, se il certificato SSL non corrisponde, la finestra di dialogo di conferma è leggermente diversa dalla finestra di dialogo visualizzata quando si usa l'opzione IIS locale.

![Avviso di sicurezza del sito remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: avviso di sicurezza del sito remoto

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 introduce l'opzione per la creazione di siti Web tramite FTP. Quando si utilizza questa opzione, l'IDE crea i file localmente nella cartella temporanea Users e quindi utilizza FTP per spostare i file nel percorso FTP.

> [!NOTE]
> Il percorso della cartella temporanea è c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;server&gt;/_&lt;nome dell'applicazione&gt;

Quando si usa l'opzione FTP, viene visualizzata una finestra di dialogo Scegli percorso. Immettere le informazioni di connessione FTP richieste in questa finestra di dialogo come illustrato di seguito.

![La finestra di dialogo Scegli percorso per FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: finestra di dialogo Scegli percorso per FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Lab: configurare il sito FTP e creare un progetto

I passaggi seguenti configurano il sito FTP in modo che un utente disponga di un percorso che può essere caricato solo tramite FTP.

### <a name="install-the-ftp-service"></a>Installare il servizio FTP

1. Aprire Aggiungi Rimuovi programmi, selezionare Installazione componenti di Windows
2. Selezionare Internet Information Services (server applicazioni in Windows 2003) e fare clic su **Dettagli**.
3. Controllare **File Transfer Protocol servizio (FTP)** e fare clic su **OK**.
4. Fare clic su **Avanti** per installare il servizio FTP.

### <a name="create-a-new-folder-for-content"></a>Crea una nuova cartella per il contenuto

1. In Esplora risorse creare una nuova cartella denominata **User1** all'interno di c:/Inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurare le cartelle e le autorizzazioni per le cartelle.

1. Aprire lo snap-in Internet Information Services da strumenti di amministrazione. A questo punto si disporrà di una cartella siti FTP nel nodo nome computer.
2. Espandere **siti FTP**.
3. Fare clic con il pulsante destro del mouse sul **sito FTP predefinito**, scegliere **nuovo**, **directory virtuale**, quindi fare clic su **Avanti**.
4. Immettere **User1** per il nome della directory virtuale e fare clic su **Avanti**.
5. Immettere **c:/Inetpub/wwwroot/User1** per il percorso e fare clic su **Avanti**.
6. Fare clic su **Avanti** e quindi su **fine** per completare la procedura guidata.
7. Fare clic con il pulsante destro del mouse sulla directory virtuale **User1** nel sito FTP predefinito e scegliere **Proprietà**.
8. Selezionare la casella di controllo **Scrivi** e fare clic su **OK** per chiudere la finestra di dialogo.
9. Fare clic con il pulsante destro del mouse su **sito FTP predefinito** e scegliere **Proprietà**.
10. Nella scheda **account di sicurezza** deselezionare **Consenti connessioni anonime**.
11. Fare clic su **Sì** nella finestra di dialogo in cui viene chiesto se si desidera continuare.
12. Fare clic su **OK** per chiudere la finestra di dialogo.
13. Espandere il **sito Web predefinito** nel nodo **siti Web** .
14. Fare clic con il pulsante destro del mouse sulla directory **User1** e scegliere **Proprietà** .
15. Nella sezione **Impostazioni applicazione** , fare clic su **Crea** per contrassegnare la cartella come applicazione.
16. Fare clic su **OK** per chiudere la finestra di dialogo.
17. Chiudere lo snap-in Internet Information Services.

### <a name="create-web-project"></a>Crea progetto Web

1. Aprire Visual Studio 2005.
2. Scegliere **nuovo sito Web**dal menu **file** .
3. Nell'elenco a discesa **location** selezionare **FTP**.
4. Fare clic su **Sfoglia**.
5. Immettere **localhost** nella casella di testo **Server** .
6. Immettere **User1** nella casella di testo Directory.
7. Fare clic su **Apri**. Il percorso FTP verrà immesso nella finestra di dialogo nuovo sito Web.
8. Fare clic su **OK**.
9. Deselezionare **accesso anonimo** nella finestra di dialogo accesso FTP, immettere le credenziali e fare clic su **OK**.
10. Qual è l'URL per il progetto? L'URL del progetto verrà visualizzato in Esplora soluzioni.
11. Scegliere **Compila sito Web** o **Compila soluzione**dal menu **Compila** .
12. Fare clic con il pulsante destro del mouse su default. aspx in Esplora soluzioni e selezionare **Visualizza nel browser**.
13. Nella finestra di dialogo URL sito Web necessario immettere `http://localhost/user1` per l'URL e fare clic su **OK**.

> [!NOTE]
> Se viene ricevuto un errore che indica l'impossibilità di caricare il tipo o _Default, assicurarsi di eseguire ASP.NET 2,0 nel sito Web e non in una versione precedente. Questa operazione può essere eseguita dalla scheda ASP.NET in Internet Information Services.

## <a name="opening-web-projects"></a>Apertura di progetti Web

L'apertura di progetti web è simile alla creazione di progetti. Le sezioni seguenti descrivono le aree per tenere d'occhio il funzionamento all'interno dell'IDE. Viene inoltre illustrata l'utilizzo di progetti Web tramite HTTP e FTP.

Per aprire un progetto Web, scegliere Apri sito Web dal menu file. Verrà visualizzata la stessa finestra di dialogo Scegli percorso analizzata in precedenza e si avranno a disposizione le stesse quattro opzioni: file System, IIS locale, FTP e sito remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>File system

Come indicato in precedenza in questo modulo, Visual Studio non usa più un file di progetto. Pertanto, se si sceglie di aprire un sito Web dalla file system, è possibile scegliere qualsiasi cartella desiderata, anche se la cartella scelta non è stata creata inizialmente come progetto Web in Visual Studio. Ad esempio, è possibile scegliere di aprire la cartella documenti come un sito Web e Visual Studio la aprirà con gioia e visualizzerà i file come illustrato di seguito.

![Documenti aperti come sito Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *documenti* aperti come sito Web

Poiché Visual Studio crea solo file e cartelle aggiuntivi quando necessario, non viene aggiunto alcun file o cartella aggiuntiva al percorso aperto. Un effetto collaterale di questa architettura è che impedisce di annidare i siti Web nel file system. Si consideri, ad esempio, la seguente struttura di directory.

Progetto Web in C:/sito Web

Un altro progetto Web in C:/sito Web/annidato

Quando si apre il sito Web in c:/sito Web, la cartella nidificata verrà visualizzata come una sottocartella dell'applicazione.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Quando si aprono siti Web tramite HTTP, le impostazioni vengono lette dalla metabase IIS (IIS locale) o utilizzando Estensioni del server di FrontPage (sito remoto). Se sono presenti applicazioni Web annidate, queste vengono visualizzate anche con un'icona che li identifica come un'applicazione. Se si ha familiarità con l'uso di applicazioni Web in FrontPage, il comportamento in Visual Studio 2005 è simile.

Anche se in Visual Studio verrà visualizzata un'icona per le applicazioni annidate sotto l'applicazione attualmente aperta nell'IDE, non sarà possibile espanderle per visualizzarne il contenuto. È tuttavia possibile fare doppio clic su di essi per aprirli. Quando si esegue questa operazione, viene visualizzata una finestra di dialogo in cui viene chiesto di aprire l'applicazione Web (e sostituire la soluzione attualmente aperta) o di aggiungere l'applicazione Web alla soluzione corrente.

![Facendo doppio clic sull'icona di un'applicazione annidata viene visualizzata questa finestra di dialogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: fare doppio clic sull'icona di un'applicazione annidata Visualizza questa finestra di dialogo

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sito FTP

Quando si apre un sito tramite FTP, tutti i file vengono copiati localmente nella cartella temporanea. Il percorso completo della posizione di archiviazione locale viene visualizzato nel riquadro proprietà del progetto e viene creato utilizzando il formato seguente.

C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;server&gt;/_&lt;nome dell'applicazione&gt;

Quando si usa FTP, Visual Studio dovrà specificare l'URL di base per il progetto in modo che sia possibile esplorarlo come illustrato di seguito. Se non si specifica un URL di base, Visual Studio chiederà se è la prima volta che si tenta di esplorare una pagina nel sito Web.

![Specifica di un URL di base per i siti FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: specifica di un URL di base per i siti FTP

## <a name="improvements-in-compilation"></a>Miglioramenti nella compilazione

L'utilizzo di applicazioni Web in Visual Studio 2005 è notevolmente più veloce rispetto alle versioni precedenti. Ciò è dovuto a una piccola parte delle modifiche nell'architettura di compilazione.

In Visual Studio 2002 e 2003 le applicazioni Web sono state compilate in un assembly primario che risiede nella cartella/bin. In Visual Studio 2005 è stata aggiunta una cartella app/_Code. Le classi e altro codice non dell'interfaccia utente vengono aggiunti alla cartella app/_Code. Quando Visual Studio compila il progetto, tutti i file nella cartella app/_Code vengono compilati in un singolo file app/_Code. dll. Il risultato di questa modifica è che le compilazioni successive sono molto più veloci rispetto alle versioni precedenti.

> [!NOTE]
> L'utilità della riga di comando MSBuild può essere usata anche per compilare applicazioni Web ASP.NET. Tale strumento verrà trattato nel modulo 9.

Un altro miglioramento della compilazione è l'opzione nuova pagina Compila del menu Compila. Questa funzionalità consente agli sviluppatori di ricompilare solo la pagina corrente (insieme alle dipendenze, ovviamente e), in modo che le modifiche possano essere compilate più rapidamente. Poiché C# in non è disponibile la compilazione in background per l'aggiornamento di IntelliSense e così via, i vantaggi offerti da questa funzionalità sono molto utili perché consente l'aggiornamento rapido di IntelliSense semplicemente ricompilando una singola pagina.

Le proprietà di compilazione per un progetto consentono di configurare il tipo di compilazione che si verifica prima dell'esecuzione della pagina di avvio. Gli sviluppatori possono scegliere di creare solo la pagina corrente in modo che Visual Studio possa avviare il debug delle applicazioni più rapidamente dopo la modifica del codice.

![Azione di avvio della pagina di compilazione](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: azione di avvio della pagina di compilazione

Un altro ottimo miglioramento di Visual Studio e dell'architettura ASP.NET è nell'area di modifica e continuazione. In Visual Studio 2005 gli sviluppatori possono avviare il debug di un progetto e apportare modifiche al codice nel progetto senza scollegare il debugger. Infatti, è possibile avviare letteralmente il debug di un progetto, aggiungere una nuova classe, aggiungere codice a tale classe, aggiungere codice alla pagina che crea una nuova istanza della classe ed eseguire un metodo della classe senza scollegare il debugger. L'esecuzione del nuovo codice è letteralmente semplice quanto l'aggiornamento del browser.

Fare clic qui per visualizzare una procedura dettagliata video della funzionalità modifica e continuazione in Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La funzionalità di modifica e continuazione affidabile in ASP.NET 2,0 e Visual Studio 2005 è causata da una modifica dell'architettura per le applicazioni ASP.NET. In ASP.NET 1. x, le applicazioni create in Visual Studio 2002/2003 sono state compilate in un assembly primario archiviato nella cartella/bin. Tutte le classi, le pagine e così via per l'applicazione sono state compilate in una DLL. Quindi, in fase di esecuzione, ASP.NET compilerà tutti i controlli, il markup e il codice ASP.NET nelle pagine e copierà tali dll nella cartella temporanea ASP.NET.

In Visual Studio 2005 usando ASP.NET 2,0, i due modelli di compilazione illustrati in precedenza (uno per Visual Studio e uno per ASP.NET in fase di esecuzione) sono Stati Uniti in un unico modello di compilazione comune. Ciò significa che tutti i problemi di compilazione vengono ora rilevati durante la fase di sviluppo anziché in fase di esecuzione. Consente inoltre il supporto di progettazione e IntelliSense per le funzionalità quali i controlli utente e le pagine master.

Fare clic qui per visualizzare una procedura dettagliata video del supporto della finestra di progettazione per i controlli utente.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Quando un controllo utente viene rimosso da una pagina, la direttiva @Register rimane nel markup e deve essere rimossa manualmente per evitare errori del parser se il controllo utente viene eliminato dal sito Web.

Un altro miglioramento del modello di compilazione di Visual Studio è la funzionalità Pubblica sito Web. Poiché la funzionalità di pubblicazione precompila il sito Web, gli sviluppatori possono sfruttare le prestazioni aggiuntive per non dover compilare alcun elemento su richiesta. Inoltre, precompila tutto il codice sorgente nella cartella app/_Code in una DLL in modo che non sia necessario distribuire codice sorgente.

![Finestra di dialogo Pubblica sito Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: finestra di dialogo Pubblica sito Web

> [!NOTE]
> L'utilità ASPNET/_compile. exe può essere usata anche per pre-compilare un'applicazione Web ASP.NET. Tale strumento verrà trattato nel modulo 9.

Quando si pubblica un sito Web, i file precompilati vengono archiviati nella cartella ASP.NET file temporanei, come illustrato di seguito. I file con estensione *compilata* sono file XML che definiscono le dipendenze per specifiche dll. Tutti i controlli WebForm o utente vengono compilati in dll casuali che iniziano con *app/_Web/_* .

Se si lascia selezionata la casella di controllo *Consenti a questo sito precompilato di essere aggiornabile* , il markup all'interno dei Web Form e dei controlli utente non verrà precompilato in una dll che consente di apportare modifiche dopo la distribuzione. Se si preferisce bloccare il markup in modo che le modifiche apportate al contenuto distribuito non siano consentite, deselezionare questa casella.

La casella di controllo *Use Fixed Naming and single page Assemblies* consente di disabilitare la compilazione batch in modo che ogni pagina venga compilata in un assembly con nome fisso. Se si lascia questa casella deselezionata, sarà possibile sfruttare la compilazione batch.

La casella di controllo *Abilita denominazione sicura su assembly precompilati* consente di assegnare un nome sicuro agli assembly precompilati.

> [!NOTE]
> In ASP.NET 1. x è stato necessario installare gli assembly con nome sicuro nella global assembly cache (GAC). In ASP.NET 2,0 non è necessario installare assembly con nome sicuro nella global assembly cache (GAC).

![File precompilati di applicazioni ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: file precompilati di applicazioni ASP.NET

> [!NOTE]
> Nell'applicazione precedente non era presente alcun file Web. config. In caso contrario, sarebbe stato chiamato *PreCompiledApp. config* dopo il processo di pubblicazione del sito Web.

## <a name="improvements-in-deployment"></a>Miglioramenti della distribuzione

Come con Visual Studio 2002 e 2003, Visual Studio 2005 offre una funzionalità di copia del progetto. Tuttavia, la funzionalità è stata rinforzata in Visual Studio 2005 ed è ora denominata Copia sito Web.

La finestra di dialogo Copia sito Web è divisa in un frame sinistro e in un frame destro. Il frame di sinistra è denominato sito Web di origine e il frame destro è denominato sito Web remoto. Una cosa che può confondere alcuni sviluppatori è che il sito visualizzato nel frame destro non è necessariamente un sito remoto. Potrebbe trattarsi di un sito sulla file system locale o sull'istanza locale di IIS. Inoltre, il sito visualizzato nel frame di sinistra non è necessariamente il sito Web di origine perché la finestra di dialogo consente di pubblicare dal sito Web remoto al sito Web *di* origine.

Se si copia un progetto in un sito Web remoto, è necessario che nel sito sia installato il Estensioni del server di FrontPage. In caso contrario, sarà necessario connettersi mediante FTP. D'altra parte, se si copia un progetto nell'istanza IIS locale, Estensioni del server di FrontPage non sono necessari.

> [!NOTE]
> Se si tenta di creare un nuovo sito Web nell'istanza IIS locale e vengono installate le estensioni del server di FrontPage 2002, verrà ricevuto un messaggio di errore che informa che la creazione di siti Web non è supportata in un server SharePoint. In tal caso, è possibile installare le estensioni del server di FrontPage 2000 o rimuovere il Estensioni del server di FrontPage.

Fare clic qui per una procedura dettagliata video della funzionalità Copia sito Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Miglioramenti del debug

Per il debug in Visual Studio 2005 sono disponibili quattro principali miglioramenti.

- Il debug locale come non amministratore è possibile.
- L'attributo debug per l'elemento compilation è ora false per impostazione predefinita.
- La configurazione e la configurazione del debug remoto sono più semplici che precedenti.
- È ora possibile eseguire il debug di un sito Web aperto tramite un percorso FTP.

## <a name="debugging-as-a-non-administrator"></a>Debug come non amministratore

L'aggiunta del Server di sviluppo ASP.NET consente a non amministratori di eseguire facilmente il debug di applicazioni ASP.NET direttamente. Quando un'applicazione ASP.NET in esecuzione nel file system locale viene sottoposta a debug, Visual Studio avvia il Server di sviluppo ASP.NET nel contesto dell'utente connesso. Tale utente potrà quindi eseguire il debug dell'applicazione senza alcuna configurazione aggiuntiva.

## <a name="debug-is-false-by-default"></a>Il debug è false per impostazione predefinita

In ASP.NET 1. x, l'attributo *debug* nell'elemento *compilation* del file Web. config è stato impostato su *true* per impostazione predefinita. È sempre consigliabile che gli sviluppatori impostino questo attributo su *false* prima di distribuire un'applicazione in produzione, ma poiché la maggior parte degli sviluppatori non è in grado di comprendere completamente le conseguenze dell'impostazione dell'attributo di debug su true, è sufficiente lasciarla così com'è.

Il problema più grave nell'impostare l'attributo debug su true è che disabilita il modello di compilazione batch ASP. NETs. Ogni pagina viene quindi compilata in una DLL separata. Se un'applicazione Web è costituita da migliaia di pagine (senza alcun mezzo), significa che verranno create diverse migliaia di dll di piccole dimensioni da tale applicazione. Anche se queste dll hanno dimensioni ridotte, non vengono caricate in una posizione specifica in memoria. Pertanto, provocano la frammentazione nella memoria di sistema e possono contribuire alle occorrenze di OutOfMemoryException.

In ASP.NET 2,0, l'attributo debug è impostato su false per impostazione predefinita. Come già visto, quando uno sviluppatore esegue il debug di un'applicazione ASP.NET in Visual Studio 2005, viene richiesto di aggiungere un file Web. config con il debug abilitato. In questo modo si verificano gli stessi svantaggi presenti in ASP.NET 1. x, ma ora lo sviluppatore viene avvertito chiaramente che l'attributo deve essere reimpostato su false prima di trasferire l'applicazione in produzione.

## <a name="remote-debugging-setup-and-configuration"></a>Configurazione e configurazione del debug remoto

In Visual Studio 2002/2003 il debug remoto si basava sulla gestione del debug del computer (MDM. exe) e sul processo Vs7jit. exe. Per questo motivo, la risoluzione dei problemi di debug remoto era spesso una black box per i clienti e spesso non era molto migliore per PSS.

Visual Studio 2005 rimuove la dipendenza dai processi MDM. exe e Vs7jit. exe. USA invece il servizio Remote Debug Monitor (msvsmon. exe).

La necessità di eseguire il debug in Visual Studio 2005 in remoto è piuttosto semplice. Prima di eseguire il debug, è necessario eseguire msvsmon. exe nel server remoto. È possibile installare Remote Debug Monitor dal CD di Visual Studio oppure è sufficiente eseguire msvsmon. exe da una condivisione senza installare alcun elemento sul server Web.

Quando si esegue msvsmon. exe, è probabile che si riferisca alle porte bloccate per il debug remoto. Fortunatamente, è possibile sbloccare facilmente le porte direttamente dall'interno della finestra di dialogo di avviso, come illustrato di seguito.

![Notifica che Windows Firewall blocca il debug remoto](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notifica che Windows Firewall blocca il debug remoto

Una volta sbloccate le porte necessarie per il debug, viene visualizzato il Remote Debugging Monitor come illustrato di seguito. Da questa interfaccia è possibile monitorare facilmente le connessioni e modificare le autorizzazioni di debug.

![Remote Debugging Monitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: Remote Debugging Monitor

È anche possibile eseguire il debug remoto di un'applicazione Web aperta tramite FTP. I passaggi sono identici a quelli descritti in precedenza. Tuttavia, sarà necessario specificare un URL di base per esplorare il progetto FTP, come descritto in precedenza in questo modulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Debug remoto con Visual Studio 2005

In questo laboratorio verrà illustrato il debug remoto con Visual Studio 2005.

Fare clic qui per una procedura dettagliata video di questo Lab.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Apri video a schermo intero](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Per questo Lab è necessario disporre di due macchine, una con Visual Studio 2005 e l'altra con IIS 5 o versione successiva.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web nel server remoto.

> [!NOTE]
> È possibile creare il sito Web in un'istanza remota di IIS o tramite FTP.

1. Dal server Web remoto individuare msvsmon. exe nel computer di sviluppo usando un percorso UNC ed eseguirlo.  
 Il percorso predefinito per msvsmon. exe è//server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/remote debugger/x86.
2. Se viene richiesto di sbloccare le porte per il debug remoto, eseguire questa operazione.
3. Dal computer di sviluppo aprire il code-behind per default. aspx e impostare un punto di interruzione nel metodo Page/_Load.
4. Avviare il debug dal computer di sviluppo.

È necessario raggiungere il punto di interruzione come previsto.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Come è già stato illustrato, Visual Studio 2005 viene fornito con un server Web denominato Server di sviluppo ASP.NET. Il Server di sviluppo ASP.NET viene talvolta definito Cassini. Questo server Web è un modo pratico per esplorare ed eseguire il debug di applicazioni Web in esecuzione nel file system.

Il Server di sviluppo ASP.NET è un server Web con restrizioni. Non consente le connessioni remote, ma non consente le richieste da alcun utente diverso dall'utente che ha avviato il server Web. Non è inoltre in grado di servire le pagine ASP. Vengono servite solo le risorse ASP.NET e le risorse HTML, incluse le immagini, i file CSS e così via.

Il Server di sviluppo ASP.NET può essere avviato tramite la riga di comando eseguendo il file WebDev. webserver. exe che si trova in c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*. Nella finestra di dialogo seguente vengono visualizzati i parametri disponibili.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**

> [!NOTE]
> Il Server di sviluppo ASP.NET non è supportato quando viene avviato in modo esplicito tramite la riga di comando.
