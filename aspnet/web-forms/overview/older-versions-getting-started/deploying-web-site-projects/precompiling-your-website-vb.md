---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Precompilazione del sito Web (VB) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: progetti di applicazione Web (WAP) e progetti di siti Web (WSPs). Una delle differenze principali tra...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 1a46870b73f95300ef5bc1f72dda74d7d62ab11f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627848"
---
# <a name="precompiling-your-website-vb"></a>Precompilazione del sito Web (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: progetti di applicazione Web (WAP) e progetti di siti Web (WSPs). Una delle differenze principali tra i due tipi di progetto è che WAP deve avere il codice compilato in modo esplicito prima della distribuzione, mentre il codice in un WSP può essere compilato automaticamente sul server Web. Tuttavia, è possibile precompilare un WSP prima della distribuzione. Questa esercitazione illustra i vantaggi della precompilazione e Mostra come precompilare un sito Web dall'interno di Visual Studio e dalla riga di comando.

## <a name="introduction"></a>Introduzione

Visual Studio offre agli sviluppatori ASP.NET due diversi tipi di progetto: Web Application Projects (WAP) e progetti di siti Web (WSP). Una delle differenze principali tra questi tipi di progetto è che WAP richiede una *compilazione esplicita* , mentre WSPs usa la *compilazione automatica*, per impostazione predefinita. Con WAP, si compila il codice dell'applicazione Web in un unico assembly, che viene creato nella cartella `Bin` del sito Web. La distribuzione comporta la copia del contenuto di markup (i file `.aspx.ascx`e `.master`) nel progetto, insieme all'assembly nella cartella `Bin` non è necessario distribuire i file della classe code-behind. D'altra parte, si distribuisce WSPs copiando le pagine di markup e le classi code-behind corrispondenti nell'ambiente di produzione. Le classi code-behind vengono compilate su richiesta sul server Web.

> [!NOTE]
> Per ulteriori informazioni sulle differenze tra i modelli di progetto, la compilazione esplicita e automatica e il modo in cui il modello di compilazione influiscono sulla distribuzione, vedere la sezione "compilazione esplicita e compilazione automatica" nell' [esercitazione *determinazione dei file da distribuire* ](determining-what-files-need-to-be-deployed-vb.md) .

L'opzione di compilazione automatica è semplice da usare. Non esiste alcuna fase di compilazione esplicita e solo i file che sono stati modificati devono essere distribuiti, mentre la compilazione esplicita richiede la distribuzione delle pagine di markup modificate e dell'assembly appena compilato. Tuttavia, la distribuzione automatica presenta due potenziali svantaggi:

- Poiché le pagine devono essere compilate automaticamente quando vengono visitate per la prima volta, è possibile che si verifichi un ritardo breve ma evidente quando viene richiesta una pagina ASP.NET per la prima volta dopo la distribuzione.
- Per la compilazione automatica è necessario che il markup dichiarativo e il codice sorgente siano presenti sul server Web. Questo può essere un'opzione non interessante se si prevede di vendere l'applicazione Web ai clienti che lo installeranno nei server Web.

Se una delle due limitazioni precedenti è costituita da breaker di gestione, è possibile passare al modello WAP o *precompilare* il WSP prima della distribuzione. In questa esercitazione vengono esaminate le opzioni di precompilazione più adatte per un sito Web ospitato e viene illustrato il processo di precompilazione e la distribuzione di un sito Web precompilato.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Panoramica della generazione e della compilazione di codice ASP.NET

Prima di esaminare le opzioni di precompilazione disponibili, è prima di tutto necessario descrivere la generazione del codice e la compilazione che si verificano quando viene richiesta una pagina ASP.NET per la prima volta dopo la creazione o l'ultimo aggiornamento. Come è noto, le pagine ASP.NET sono costituite da due parti: markup dichiarativo nel file `.aspx`; e una parte del codice sorgente, in genere in un file di classe code-behind separato (`.aspx.vb`). I passaggi eseguiti dal runtime quando viene richiesta una pagina ASP.NET dipendono dal modello di compilazione dell'applicazione.

Con WAP, il codice sorgente delle pagine deve essere compilato in modo esplicito in un singolo assembly prima di essere distribuito. Durante la distribuzione, l'assembly e le varie pagine di markup vengono copiate nell'ambiente di produzione. Quando una richiesta arriva al server Web per una pagina ASP.NET, il runtime crea un'istanza della classe code-behind della pagina e richiama il metodo `ProcessRequest`, che avvia il ciclo di vita della pagina e, infine, genera il contenuto della pagina, che viene restituito al richiedente. Il runtime può funzionare con la classe code-behind della pagina ASP.NET perché la classe code-behind è già stata compilata in un assembly prima della distribuzione.

Con WSPs e la compilazione automatica, non esiste alcuna fase di compilazione esplicita prima della distribuzione. La distribuzione comporta invece la copia del contenuto dichiarativo e del codice sorgente nell'ambiente di produzione. Quando una richiesta arriva al server Web per una pagina ASP.NET per la prima volta dopo la creazione o l'ultimo aggiornamento della pagina, il runtime deve prima compilare la classe code-behind in un assembly. Questo assembly compilato viene salvato nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, anche se il percorso di questa cartella può essere personalizzato tramite l'elemento `<pages>` in `Web.config`. Poiché l'assembly viene salvato su disco, non è necessario ricompilarlo nelle richieste successive alla stessa pagina.

> [!NOTE]
> Come ci si aspetterebbe, si verifica un lieve ritardo quando si richiede una pagina per la prima volta (o per la prima volta dopo che è stata modificata) in un sito che usa la compilazione automatica, perché richiede un attimo al server di compilare il codice della pagina e salvare l'assembly risultante in disco.

In breve, con la compilazione esplicita è necessario compilare il codice sorgente del sito Web prima della distribuzione, evitando al runtime di dover eseguire tale passaggio. Con la compilazione automatica il runtime gestisce la compilazione del codice sorgente delle pagine, ma con un lieve costo di inizializzazione per la prima visita alla pagina dopo la creazione o l'ultimo aggiornamento.

Ma la parte dichiarativa delle pagine ASP.NET (il file `.aspx`)? È ovvio che esiste una relazione tra i file di `.aspx` e il codice nelle rispettive classi code-behind, perché i controlli Web definiti nel markup dichiarativo sono accessibili nel codice. È anche ovvio che il contenuto nei file di `.aspx` influenza significativamente il markup sottoposto a rendering generato dalla pagina. Quindi, in che modo il runtime funziona con la sintassi di testo, HTML e controllo Web definita nel file di `.aspx` per generare il contenuto sottoposto a rendering della pagina richiesta?

Non voglio troppo distrarre i dettagli di implementazione di basso livello, che variano tra WAP e WSPs, ma in breve il runtime genera automaticamente un file di classe che contiene i vari controlli Web come membri e metodi protetti. Questo file generato viene implementato come *classe parziale* nella classe code-behind corrispondente. ([Le classi parziali](http://www.dotnet-guide.com/partialclasses.html) consentono di distribuire il contenuto di una singola classe in più file.) La classe code-behind viene pertanto definita in due posizioni: nel file di `.aspx.vb` creato e in questa classe generata automaticamente creata dal runtime. Questa classe generata automaticamente viene archiviata nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`.

Il punto importante è che, per eseguire il rendering di una pagina ASP.NET dal runtime, è necessario compilare in un assembly le parti dichiarative e di codice sorgente. Con WAP, il codice sorgente viene compilato in modo esplicito in un assembly prima della distribuzione, ma il markup dichiarativo deve comunque essere convertito nel codice e compilato dal runtime sul server Web. Con WSPs tramite la compilazione automatica, il codice sorgente e il markup dichiarativo devono essere compilati dal server Web.

È possibile usare la compilazione esplicita con il modello WSP. È possibile compilare in modo esplicito la parte del codice sorgente, ad esempio con il modello WAP. Inoltre, è possibile compilare il markup dichiarativo.

## <a name="precompilation-options"></a>Opzioni di precompilazione

Il .NET Framework viene fornito con uno [strumento di compilazione ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) che consente di compilare il codice sorgente (e anche il contenuto) di un'applicazione ASP.NET compilata utilizzando il modello WSP. Questo strumento è stato rilasciato con la versione .NET Framework 2,0 e si trova nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`; può essere usato dalla riga di comando o avviato dall'interno di Visual Studio tramite l'opzione Pubblica sito Web del menu Compila.

Lo strumento di compilazione fornisce due forme generali di compilazione: precompilazione e precompilazione sul posto per la distribuzione. Con la precompilazione sul posto si esegue lo strumento `aspnet_compiler.exe` dalla riga di comando e si specifica il percorso della directory virtuale o del percorso fisico di un sito Web che risiede nel computer. Lo strumento di compilazione compila quindi ogni pagina ASP.NET del progetto, archiviando la versione compilata nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` esattamente come se le pagine fossero state visitate per la prima volta da un browser. La precompilazione sul posto può velocizzare la prima richiesta effettuata alle pagine ASP.NET appena distribuite nel sito, in quanto attenua il runtime dalla necessità di eseguire questo passaggio. Tuttavia, la precompilazione sul posto non è utile per la maggior parte dei siti Web ospitati perché richiede che sia possibile eseguire programmi dalla riga di comando del server Web. Negli ambienti host condivisi questo livello di accesso non è consentito.

> [!NOTE]
> Per altre informazioni sulla precompilazione sul posto, vedere [procedura: precompilare i siti Web ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) e [precompilazione in ASP.NET 2,0](http://www.odetocode.com/Articles/417.aspx).

Anziché compilare le pagine del sito Web nella cartella `Temporary ASP.NET Files`, la precompilazione per la distribuzione compila le pagine in una directory di propria scelta e in un formato che può essere distribuito nell'ambiente di produzione.

Esistono due tipi di precompilazione per la distribuzione che verranno analizzati in questa esercitazione: precompilazione con un'interfaccia utente aggiornabile e precompilazione con un'interfaccia utente non aggiornabile. La precompilazione con un'interfaccia utente aggiornabile lascia il markup dichiarativo nei file di `.aspx`, `.ascx`e `.master`, consentendo allo sviluppatore di visualizzare e, se necessario, di modificare il markup dichiarativo nel server di produzione. La precompilazione con un'interfaccia utente non aggiornabile genera `.aspx` pagine prive di contenuto e rimuove i file di `.ascx` e `.master`, nascondendo quindi il markup dichiarativo e impedendo a uno sviluppatore di modificarlo dall'ambiente di produzione.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente aggiornabile

Il modo migliore per comprendere la precompilazione per la distribuzione è vedere un esempio in azione. Per la distribuzione, è precompilare le revisioni del libro WSP usando un'interfaccia utente aggiornabile. Lo strumento di compilazione ASP.NET può essere richiamato dal menu Compila di Visual Studio o dalla riga di comando. In questa sezione viene esaminato l'utilizzo dello strumento da Visual Studio. la sezione "precompilazione dalla riga di comando" esamina l'esecuzione dello strumento compilatore dalla riga di comando.

Aprire il WSP Book Review in Visual Studio, passare al menu Compila e selezionare l'opzione di menu Pubblica sito Web. Verrà avviata la finestra di dialogo Pubblica sito Web (vedere la **Figura 1**), in cui è possibile specificare il percorso di destinazione, indipendentemente dal fatto che l'interfaccia utente del sito precompilato sia aggiornabile e altre opzioni dello strumento del compilatore. Il percorso di destinazione può essere un server Web remoto o un server FTP, ma per ora scegliere una cartella sul disco rigido del computer. Poiché si desidera precompilare il sito con un'interfaccia utente aggiornabile, lasciare selezionata la casella di controllo "Consenti a questo sito precompilato" di essere aggiornabile e fare clic su OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: lo strumento di compilazione ASP.NET consente di precompilare il sito Web nel percorso di destinazione specificato  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> L'opzione Pubblica sito Web nel menu Compila non è disponibile in Visual Web Developer. Se si utilizza Visual Web Developer, sarà necessario utilizzare la versione da riga di comando dello strumento di compilazione ASP.NET, descritto nella sezione "precompilazione dalla riga di comando".

Dopo aver precompilato il sito Web, passare al percorso di destinazione immesso nella finestra di dialogo Pubblica sito Web. Confrontare il contenuto di questa cartella con il contenuto del sito Web. **Nella figura 2** è illustrata la cartella Book revisioni del sito Web. Si noti che contiene sia `.aspx` che i file di `.aspx.cs`. Si noti inoltre che la directory `Bin` include un solo file, `Elmah.dll`, che è stato aggiunto nell' [esercitazione precedente](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: la directory del progetto contiene file di `.aspx` e di `.aspx.cs`; la cartella `Bin` include solo `Elmah.dll`  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](precompiling-your-website-vb/_static/image6.png))

**Nella figura 3** viene illustrata la cartella del percorso di destinazione il cui contenuto è stato creato dallo strumento di compilazione ASP.NET. Questa cartella non contiene file code-behind. Inoltre, la directory `Bin` della cartella include diversi assembly e due file di `.compiled` oltre all'assembly di `Elmah.dll`.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: la cartella del percorso di destinazione include i file per la distribuzione  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](precompiling-your-website-vb/_static/image9.png))

A differenza della compilazione esplicita in WAP, la precompilazione per il processo di distribuzione non crea un assembly per l'intero sito. Al contrario, raggruppa diverse pagine in ogni assembly. Compila inoltre il file di `Global.asax` (se presente) nel relativo assembly, nonché qualsiasi classe nella cartella `App_Code`. I file che contengono il markup dichiarativo per le pagine Web ASP.NET, i controlli utente e le pagine master (rispettivamente`.aspx`, `.ascx`e `.master`) vengono copiati così come sono nella directory del percorso di destinazione. Analogamente, il file di `Web.config` viene copiato direttamente, insieme a qualsiasi file statico, ad esempio immagini, classi CSS e file PDF. Per una descrizione più formale del modo in cui lo strumento di compilazione gestisce diversi tipi di file, vedere [gestione dei file durante la precompilazione di ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> È possibile indicare allo strumento di compilazione di creare un assembly per ogni pagina di ASP.NET, controllo utente o pagina master selezionando la casella di controllo "utilizzo della denominazione fissa e degli assembly a pagina singola" nella finestra di dialogo Pubblica sito Web. Ogni pagina ASP.NET compilata nel proprio assembly consente un controllo più granulare sulla distribuzione. Se, ad esempio, è stata aggiornata una singola pagina Web ASP.NET ed è necessario distribuire la modifica, è necessario distribuire solo il file di `.aspx` della pagina e l'assembly associato all'ambiente di produzione. Per ulteriori informazioni [, vedere Procedura: generare nomi fissi con lo strumento di compilazione ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) .

La directory del percorso di destinazione contiene anche un file che non faceva parte del progetto Web precompilato, ovvero `PrecompiledApp.config`. Questo file informa il runtime ASP.NET che l'applicazione è stata precompilata e se è stata precompilata con un'interfaccia utente aggiornabile o aggiornabile da mezzogiorno.

Infine, è necessario aprire uno dei file `.aspx` nel percorso di destinazione usando Visual Studio o l'editor di testo desiderato. Quando si esegue la precompilazione per la distribuzione con un'interfaccia utente aggiornabile, le pagine ASP.NET nella directory del percorso di destinazione contengono esattamente lo stesso markup dei file corrispondenti nel sito Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente non aggiornabile

Lo strumento compilatore ASP.NET può essere usato anche per precompilare un sito per la distribuzione con un'interfaccia utente non aggiornabile. La precompilazione del sito con un'interfaccia utente non aggiornabile funziona in modo molto simile alla precompilazione con un'interfaccia utente aggiornabile, la differenza principale consiste nel fatto che le pagine ASP.NET, i controlli utente e le pagine master nella directory di destinazione vengono rimossi dal markup. Per precompilare un sito Web per la distribuzione con un'interfaccia utente non aggiornabile, scegliere l'opzione Pubblica sito Web dal menu Compila, ma deselezionare l'opzione "Consenti l'aggiornabilità di questo sito precompilato" (vedere la **Figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: deselezionare l'opzione "Consenti l'aggiornabilità di questo sito precompilato" per la precompilazione con un'interfaccia utente non aggiornabile  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](precompiling-your-website-vb/_static/image12.png))

**Nella figura 5** viene illustrata la cartella del percorso di destinazione dopo la precompilazione con un'interfaccia utente non aggiornabile.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: cartella del percorso di destinazione per la distribuzione con un'interfaccia utente non aggiornabile  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](precompiling-your-website-vb/_static/image15.png))

Confrontare la **Figura 3** con la **Figura 5**. Sebbene le due cartelle risultino identiche, si noti che la cartella dell'interfaccia utente non aggiornabile non dispone della pagina master `Site.master`. Mentre **nella figura 5** sono incluse le varie pagine ASP.NET, se si Visualizza il contenuto di questi file, si noterà che sono state rimosse dal markup dichiarativo e sostituite con il testo segnaposto: "si tratta di un file marcatore generato dallo strumento di precompilazione e non deve essere eliminato!".

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: il markup dichiarativo è stato rimosso dalle pagine ASP.NET

Le cartelle `Bin` nelle **figure 3** e **5** differiscono in modo sostanziale. Oltre agli assembly, la cartella `Bin` nella **Figura 5** include un file di `.compiled` per ogni pagina ASP.NET, controllo utente e pagina master.

La precompilazione di un sito con un'interfaccia utente non aggiornabile è utile nelle situazioni in cui non si desidera che il contenuto delle pagine di ASP.NET venga modificato dalla persona o dalla società che installa o gestisce il sito Web nell'ambiente di produzione. Se si compila un'applicazione Web ASP.NET che si vende ai clienti per l'installazione nei propri server Web, è consigliabile assicurarsi che non modifichino l'aspetto del sito modificando direttamente le pagine `.aspx` fornite. Precompilando il sito Web con un'interfaccia utente non aggiornabile, si inviano le pagine segnaposto `.aspx` come parte dell'installazione, impedendo così ai clienti di esaminare o modificare il contenuto.

### <a name="precompiling-from-the-command-line"></a>Precompilazione dalla riga di comando

Dietro le quinte, la finestra di dialogo Pubblica sito Web di Visual Studio richiama lo strumento di compilazione ASP.NET (`aspnet_compiler.exe`) per precompilare il sito Web. In alternativa, è possibile richiamare questo strumento dalla riga di comando. Infatti, se si utilizza Visual Web Developer, sarà necessario eseguire lo strumento del compilatore dalla riga di comando, in quanto il menu Compila di Visual Web Developer non include l'opzione Pubblica sito Web.

Per usare lo strumento compilatore dalla riga di comando, iniziare con l'eliminazione dalla riga di comando e passare alla directory del Framework `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Immettere quindi l'istruzione seguente nella riga di comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Il comando precedente avvia lo strumento del compilatore ASP.NET (`aspnet_compiler.exe`) e, tramite l'opzione di `-p`, indica di precompilare il sito Web radice nel *percorso di\_fisico\_per\_app*; Questo valore sarà simile a `C:\MySites\BookReviews`e deve essere delimitato da virgolette.

L'opzione `-v` specifica la directory virtuale del sito. Se il sito è registrato come sito Web predefinito nella metabase IIS, è possibile omettere l'opzione `-p` e specificare solo la directory virtuale dell'applicazione. Se si usa l'opzione `-p`, il valore che continua con l'opzione di `-v` indica la radice del sito Web e viene usato per risolvere i riferimenti alla radice dell'applicazione. Se ad esempio si specifica un valore di `-v /MySite`, i riferimenti nell'applicazione a `~/path/file` verranno risolti come `~/MySite/path/file`. Poiché il sito revisioni del libro si trova nella directory radice della mia società di hosting Web, ho usato il Commuter `-v /`.

L'opzione `-f`, se presente, indica allo strumento di compilazione di sovrascrivere il *percorso di\_di destinazione\_* directory della cartella, se esiste già. Se si omette l'opzione `-f` e la cartella percorso di destinazione esiste già, lo strumento di compilazione verrà chiuso con l'errore: "Errore ASPRUNTIME: la directory di destinazione non è vuota. Eliminarlo manualmente o scegliere una destinazione diversa ".

L'opzione `-u`, se presente, informa lo strumento per la creazione di un'interfaccia utente aggiornabile. Omettere questa opzione per precompilare il sito con un'interfaccia utente non aggiornabile.

Infine, il percorso del *\_di destinazione\_cartella* è il percorso fisico della directory del percorso di destinazione; Questo valore sarà simile a `C:\MySites\Output\BookReviews`e deve essere delimitato da virgolette.

## <a name="deploying-the-precompiled-website"></a>Distribuzione del sito Web precompilato

A questo punto è stato illustrato come usare lo strumento di compilazione ASP.NET per precompilare un sito Web usando le opzioni dell'interfaccia utente aggiornabili e non aggiornabili. Tuttavia, gli esempi hanno finora precompilato il sito Web in una cartella locale e non nell'ambiente di produzione. La novità è che la distribuzione del sito Web precompilato è una brezza e può essere eseguita tramite Visual Studio o tramite un altro meccanismo di copia dei file, ad esempio da un client FTP autonomo.

La finestra di dialogo Pubblica sito Web (prima mostrata nella **Figura 1**) dispone di un'opzione percorso di destinazione, che indica la posizione in cui vengono copiati i file del sito Web precompilato. Questo percorso può essere un server Web remoto o un server FTP. L'immissione di un server remoto in questa casella di testo consente di precompilare e distribuire il sito Web nel server specificato in un unico passaggio. In alternativa, è possibile precompilare il sito Web in una cartella locale e quindi copiare manualmente il contenuto di tale cartella nell'ambiente di produzione tramite FTP o un altro approccio.

Il sito Web precompilato distribuito automaticamente tramite la finestra di dialogo Pubblica sito Web di Visual Studio è utile per siti semplici in cui non sono presenti differenze di configurazione tra gli ambienti di sviluppo e di produzione. Tuttavia, come indicato nell'esercitazione sulle [ *differenze di configurazione comuni tra sviluppo e produzione* ](common-configuration-differences-between-development-and-production-vb.md) , non è insolito che tali differenze esistano. Ad esempio, l'applicazione Web Book revisioni usa un database diverso nell'ambiente di produzione rispetto all'ambiente di sviluppo. Quando Visual Studio pubblica il sito Web in un server remoto, copia in modo cieco le informazioni del file di configurazione nell'ambiente di sviluppo.

Per i siti con differenze di configurazione tra gli ambienti di sviluppo e di produzione può essere preferibile precompilare il sito in una directory locale, copiare i file di configurazione specifici per la produzione e quindi copiare il contenuto dell'output precompilato in produzione.

Per un aggiornamento della copia dei file dall'ambiente di sviluppo all'ambiente di produzione, vedere la pagina relativa alla [*distribuzione del sito Web tramite un client FTP*](deploying-your-site-using-an-ftp-client-vb.md) e alla [*distribuzione del sito Web con*](determining-what-files-need-to-be-deployed-vb.md) le esercitazioni di Visual Studio.

## <a name="summary"></a>Riepilogo

ASP.NET supporta due modalità di compilazione: automatica ed esplicita. Come illustrato nelle esercitazioni precedenti, i progetti di applicazione Web (WAP) utilizzano la compilazione esplicita, mentre i progetti di siti Web (WSPs) utilizzano la compilazione automatica, per impostazione predefinita. Tuttavia, è possibile compilare in modo esplicito un oggetto WSP prima della distribuzione usando lo strumento di compilazione ASP.NET.

Questa esercitazione è stata incentrata sulla precompilazione dello strumento di compilazione per il supporto della distribuzione. Quando si esegue la precompilazione per la distribuzione, lo strumento di compilazione crea una cartella del percorso di destinazione, compila il codice sorgente dell'applicazione Web specificata e copia gli assembly compilati e i file di contenuto nella cartella del percorso di destinazione. Lo strumento di compilazione può essere configurato per creare un'interfaccia utente aggiornabile o non aggiornabile. Quando si esegue la precompilazione con un'opzione dell'interfaccia utente non aggiornabile, viene rimosso il markup dichiarativo nei file di contenuto. In breve, la precompilazione consente di distribuire l'applicazione basata su progetti di siti Web senza includere alcun file di codice sorgente e con il markup dichiarativo rimosso, se lo si desidera.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Precompilazione del sito Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Codebehind e compilazione in ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilazione in ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opzioni del sito precompilato in ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-elmah-vb.md)
> [Successivo](users-and-roles-on-the-production-website-vb.md)
