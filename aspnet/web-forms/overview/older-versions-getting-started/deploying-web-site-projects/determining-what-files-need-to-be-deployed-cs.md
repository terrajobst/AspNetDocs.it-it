---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Determinazione dei file da distribuire (C#) | Microsoft Docs
author: rick-anderson
description: I file che devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipendono in parte dal fatto che l'applicazione ASP.NET sia stata compilata...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 1dd4a1179d32f776626c08a07205dc9aabed588d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636178"
---
# <a name="determining-what-files-need-to-be-deployed-c"></a>Determinazione dei file da distribuire (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> I file che devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipendono in parte dal fatto che l'applicazione ASP.NET sia stata compilata utilizzando il modello di sito Web o il modello di applicazione Web. Altre informazioni su questi due modelli di progetto e sul modo in cui il modello di progetto influiscono sulla distribuzione.

## <a name="introduction"></a>Introduzione

La distribuzione di un'applicazione Web ASP.NET comporta la copia dei file correlati a ASP.NET dall'ambiente di sviluppo all'ambiente di produzione. I file correlati a ASP.NET includono il markup e il codice della pagina Web ASP.NET e i file di supporto lato client e lato server. I file di supporto lato client sono i file a cui fanno riferimento le pagine Web e inviati direttamente ai file browser-images, CSS e JavaScript, ad esempio. I file di supporto lato server includono quelli usati per elaborare una richiesta sul lato server. Sono inclusi i file di configurazione, i servizi Web, i file di classe, i DataSet tipizzati e i file di LINQ to SQL, tra gli altri.

In generale, tutti i file di supporto lato client devono essere copiati dall'ambiente di sviluppo all'ambiente di produzione, ma i file di supporto lato server vengono copiati a seconda che si stia compilando in modo esplicito il codice sul lato server in un assembly (un file di `.dll`) o in caso di generazione automatica di questi assembly. Questa esercitazione illustra i file che devono essere distribuiti quando si compila in modo esplicito il codice in un assembly rispetto a quando questo passaggio di compilazione viene eseguito automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilazione esplicita rispetto alla compilazione automatica

Le pagine Web di ASP.NET sono divise in markup dichiarativo e codice sorgente. La parte di markup dichiarativa include HTML, controlli Web e sintassi di associazione dati. la parte del codice contiene i gestori eventi scritti in Visual Basic C# o nel codice. Il markup e le parti di codice sono in genere separati in file diversi: `WebPage.aspx` contiene il markup dichiarativo mentre `WebPage.aspx.cs` ospita il codice.

Si consideri una pagina ASP.NET denominata Clock. aspx contenente un controllo etichetta la cui proprietà Text è impostata sulla data e l'ora correnti in cui viene caricata la pagina. La parte di markup dichiarativa (in `Clock.aspx`) conterrà il markup per un controllo Web etichetta-`<asp:Label runat="server" id="TimeLabel" />`-mentre la parte del codice (in `Clock.aspx.cs`) avrebbe un gestore eventi `Page_Load` con il codice seguente:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Per consentire al motore ASP.NET di servire una richiesta per questa pagina, è necessario compilare la parte di codice della pagina (il file `WebPage.aspx.cs`). Questa compilazione può essere eseguita in modo esplicito o automatico.

Se la compilazione viene eseguita in modo esplicito, il codice sorgente dell'intera applicazione viene compilato in uno o più assembly (file`.dll`) presenti nella directory `Bin` dell'applicazione. Se la compilazione viene eseguita automaticamente, l'assembly generato automaticamente risultante è, per impostazione predefinita, inserito nella cartella `Temporary ASP.NET` files, disponibile in `%WINDOWS%\Microsoft.NET\Framework\` *&lt;versione&gt;* , sebbene sia possibile configurare questo percorso tramite l' [elemento`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx) in `Web.config`. Con la compilazione esplicita è necessario eseguire alcune operazioni per compilare il codice dell'applicazione ASP.NET in un assembly e questo passaggio si verifica prima della distribuzione. Con la compilazione automatica il processo di compilazione si verifica nel server Web quando si accede per la prima volta alla risorsa.

Indipendentemente dal modello di compilazione usato, la parte relativa al markup di tutte le pagine ASP.NET (i file `WebPage.aspx`) deve essere copiata nell'ambiente di produzione. Con la compilazione esplicita è necessario copiare gli assembly nella cartella `Bin`, ma non è necessario copiare le parti di codice delle pagine ASP.NET (file di `WebPage.aspx.cs`). Con la compilazione automatica è necessario copiare i file della parte di codice in modo che il codice sia presente e possa essere compilato automaticamente quando si visita la pagina. La parte relativa al markup di ogni pagina Web ASP.NET include una direttiva `@Page` con attributi che indicano se il codice associato della pagina è già stato compilato in modo esplicito o se deve essere compilato automaticamente. Di conseguenza, l'ambiente di produzione può funzionare senza problemi con entrambi i modelli di compilazione e non è necessario applicare impostazioni di configurazione speciali per indicare che viene utilizzata la compilazione automatica o esplicita.

Nella tabella 1 vengono riepilogati i diversi file da distribuire quando si utilizza la compilazione esplicita e la compilazione automatica. Si noti che, indipendentemente dal modello di compilazione utilizzato, è consigliabile distribuire sempre gli assembly nella cartella `Bin`, se tale cartella esiste. La cartella `Bin` contiene gli assembly specifici dell'applicazione Web, che includono il codice sorgente compilato quando si utilizza il modello di compilazione esplicito. La directory `Bin` contiene anche assembly di altri progetti ed eventuali assembly open source o di terze parti che si sta usando e che devono essere presenti nel server di produzione. Pertanto, come regola empirica generale, copiare la cartella `Bin` fino alla fase di produzione durante la distribuzione. Se si utilizza il modello di compilazione automatica e non si utilizzano assembly esterni, non sarà disponibile una directory `Bin`.

| **Modello di compilazione** | **Distribuire il file della parte di markup?** | **Distribuire il file di codice sorgente?** | **Distribuire gli assembly in `Bin` directory?** |
| --- | --- | --- | --- |
| Compilazione esplicita | Yes | No | Yes |
| Compilazione automatica | Yes | Yes | Sì (se esiste) |

**Tabella 1:** I file distribuiti dipendono dal modello di compilazione utilizzato.

## <a name="taking-a-trip-down-memory-lane"></a>Esecuzione di una corsia di memoria

L'approccio di compilazione da usare dipende, in parte, dalla modalità di gestione dell'applicazione ASP.NET in Visual Studio. Poiché. NET ' s ception nell'anno 2000 sono state apportate quattro versioni diverse di Visual Studio: Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 e Visual Studio 2008. Visual Studio .NET 2002 e 2003 applicazioni ASP.NET gestite che usano il *modello di progetto di applicazione Web*. Le funzionalità principali del modello di progetto di applicazione Web sono:

- I file che comprimano il progetto sono definiti in un singolo file di progetto. Qualsiasi file non definito nel file di progetto non è considerato parte dell'applicazione Web da Visual Studio.
- Usa la compilazione esplicita. La compilazione del progetto compilerà i file di codice all'interno del progetto in un unico assembly inserito nella cartella `Bin`.

Quando Microsoft ha rilasciato Visual Studio 2005, ha eliminato il supporto per il modello di progetto di applicazione Web e lo ha sostituito con il modello di progetto di sito Web. Il modello di progetto di sito Web si è differenziato dal modello di progetto di applicazione Web nei modi seguenti:

- Invece di avere un singolo file di progetto che dichiari i file del progetto, viene usato il file system. In breve, tutti i file nella cartella dell'applicazione Web (o nelle sottocartelle) vengono considerati parte del progetto.
- La compilazione di un progetto in Visual Studio non comporta la creazione di un assembly nella directory `Bin`. Al contrario, la compilazione di un progetto di sito Web segnala eventuali errori in fase di compilazione.
- Supporto per la compilazione automatica. I progetti di siti Web vengono in genere distribuiti copiando il markup e il codice sorgente nell'ambiente di produzione, anche se il codice può essere precompilato (compilazione esplicita).

Microsoft ha riattivato il modello di progetto di applicazione Web al momento del rilascio di Visual Studio 2005 Service Pack 1. Tuttavia, Visual Web Developer continua a supportare solo il modello di progetto di sito Web. La novità è che questa limitazione è stata eliminata con Visual Web Developer 2008 Service Pack 1. Oggi è possibile creare applicazioni ASP.NET in Visual Studio (e Visual Web Developer) usando il modello di progetto di applicazione Web o il modello di progetto di sito Web. Entrambi i modelli hanno vantaggi e svantaggi. Vedere [Introduzione ai progetti di applicazione Web: confronto tra progetti di siti Web e progetti di applicazione Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) per un confronto tra i due modelli e per decidere quale modello di progetto funziona meglio per la situazione specifica.

## <a name="exploring-the-sample-web-application"></a>Esplorazione dell'applicazione Web di esempio

Il download di questa esercitazione include un'applicazione ASP.NET denominata revisioni dei libri. Il sito Web riproduce un sito Web Hobby che qualcuno potrebbe creare per condividere le proprie revisioni dei libri con la community online. Questa applicazione Web ASP.NET è molto semplice ed è costituita dalle risorse seguenti:

- `Web.config`, il file di configurazione dell'applicazione.
- Una pagina master (`Site.master`).
- Sette diverse pagine di ASP.NET: 

    - ~`/Default.aspx`: Home page del sito.
    - ~`/About.aspx`: pagina "informazioni sul sito".
    - ~`/Fiction/Default.aspx`-una pagina che elenca i libri di fiction che sono stati esaminati. 

        - ~`/Fiction/Blaze.aspx`-una recensione di Richard Bachman Novel *Blaze*.
    - ~/`Tech/Default.aspx`-una pagina che elenca i libri tecnologici che sono stati esaminati. 

        - ~/`Tech/CYOW.aspx`: una recensione di *creare il proprio sito Web*.
        - ~/`Tech/TYASP35.aspx`: una recensione di *Teach Yourself ASP.NET 3,5 in 24 ore*.
- Tre diversi file CSS nella cartella stili.
- Quattro file di immagine: un logo con tecnologia ASP.NET e immagini delle copertine dei tre libri rivisti, tutti disponibili nella cartella `Images`.
- Un file di `Web.sitemap`, che definisce la mappa del sito e viene utilizzato per visualizzare i menu nelle pagine `Default.aspx` della directory radice e `Fiction` e `Tech` cartelle.
- Un file di classe denominato `BasePage.cs` che definisce una classe `Page` di base. Questa classe estende la funzionalità della classe `Page` impostando automaticamente la proprietà `Title` in base alla posizione della pagina nella mappa del sito. In breve, qualsiasi classe code-behind ASP.NET che estende `BasePage` (anziché `System.Web.UI.Page`) avrà il relativo titolo impostato su un valore a seconda della posizione nella mappa del sito. Ad esempio, quando si visualizza la pagina ~/`Tech/CYOW.aspx`, il titolo è impostato su "Home: Technology: crea il proprio sito Web".

Nella figura 1 viene mostrata una schermata del sito Web delle recensioni dei libri quando viene visualizzata tramite un browser. Qui viene visualizzata la pagina ~/`Tech/TYASP35.aspx`, che esamina il libro *Teach Yourself ASP.NET 3,5 in 24 ore*. Il percorso di navigazione che si estende nella parte superiore della pagina e il menu nella colonna a sinistra sono basati sulla struttura della mappa del sito definita in `Web.sitemap`. L'immagine nell'angolo superiore destro è una delle immagini del libro che si trovano nella cartella `Images`. L'aspetto del sito Web viene definito tramite le regole del foglio di stile CSS digitate dai file CSS nella cartella Styles, mentre il layout della pagina principale viene definito nella pagina master `Site.master`.

[![il sito Web revisioni del libro offre recensioni su un assortimento di titoli](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** Il sito Web revisioni offre recensioni su un assortimento di titoli ([fare clic per visualizzare l'immagine con dimensioni complete](determining-what-files-need-to-be-deployed-cs/_static/image3.png))

Questa applicazione non utilizza un database. ogni revisione viene implementata come pagina Web separata nell'applicazione. Questa esercitazione (e le varie esercitazioni successive) illustra la distribuzione di un'applicazione Web che non dispone di un database. Tuttavia, in un'esercitazione futura l'applicazione verrà migliorata in modo da archiviare le revisioni, i commenti dei lettori e altre informazioni all'interno di un database e verranno esaminati i passaggi da eseguire per distribuire correttamente un'applicazione Web basata sui dati.

> [!NOTE]
> Queste esercitazioni si concentrano sull'hosting di applicazioni ASP.NET con un provider host Web e non esplorano argomenti ausiliari come ASP. Sistema di mappa del sito di NET o tramite una classe `Page` di base. Per ulteriori informazioni su queste tecnologie e per altre informazioni su altri argomenti trattati nell'esercitazione, vedere la sezione relativa alla lettura successiva alla fine di ogni esercitazione.

Il download di questa esercitazione include due copie dell'applicazione Web, ognuna implementata come un tipo di progetto di Visual Studio diverso: BookReviewsWAP, un progetto di applicazione Web e BookReviewsWSP, un progetto di sito Web. Entrambi i progetti sono stati creati con Visual Web Developer 2008 SP1 e usano ASP.NET 3,5 SP1. Per iniziare a usare questi progetti, decomprimere il contenuto sul desktop. Per aprire il progetto di applicazione Web (BookReviewsWAP), passare alla cartella BookReviewsWAP e fare doppio clic sul file di soluzione `BookReviewsWAP.sln`. Per aprire il progetto di sito Web (BookReviewsWSP), avviare Visual Studio, scegliere l'opzione Apri sito Web dal menu file, passare alla cartella `BookReviewsWSP` sul desktop e fare clic su OK.

Nelle due sezioni rimanenti di questa esercitazione vengono esaminati i file che è necessario copiare nell'ambiente di produzione quando si distribuisce l'applicazione. Le due esercitazioni successive, ovvero la *[distribuzione del sito tramite FTP](deploying-your-site-using-an-ftp-client-cs.md)* e la *[distribuzione del sito tramite Visual Studio](deploying-your-site-using-visual-studio-cs.md)* , mostrano diversi modi per copiare questi file in un provider host Web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinazione dei file da distribuire per il progetto di applicazione Web

Il modello di progetto applicazione Web usa la compilazione esplicita. il codice sorgente del progetto viene compilato in un singolo assembly ogni volta che si compila l'applicazione. Questa compilazione include i file code-behind delle pagine ASP.NET (~/`Default.aspx.cs`, ~/`About.aspx.cs`e così via), nonché la classe `BasePage.cs`. L'assembly risultante è denominato BookReviewsWAP. dll e si trova nella directory di `Bin` dell'applicazione.

Nella figura 2 sono illustrati i file che costituiscono il progetto di applicazione Web Book revisioni.

[![Esplora soluzioni elenca i file che comprendono il progetto di applicazione Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: l'Esplora soluzioni elenca i file che comprendono il progetto di applicazione Web

Per distribuire un'applicazione ASP.NET sviluppata usando il modello di progetto di applicazione Web, iniziare compilando l'applicazione in modo da compilare in modo esplicito il codice sorgente più recente in un assembly. Copiare quindi i file seguenti nell'ambiente di produzione:

- I file che contengono il markup dichiarativo per ogni pagina di ASP.NET, ad esempio ~/`Default.aspx`, ~/`About.aspx`e così via. Inoltre, copiare il markup dichiarativo per tutte le pagine master e i controlli utente.
- Gli assembly (file`.dll`) nella cartella `Bin`. Non è necessario copiare i file di database di programma (`.pdb`) o tutti i file XML che potrebbero essere presenti nella directory `Bin`.

Non è necessario copiare i file di codice sorgente delle pagine ASP.NET nell'ambiente di produzione, né copiare il file di classe `BasePage.cs`.

> [!NOTE]
> Come illustrato nella figura 2, la classe `BasePage` viene implementata come file di classe nel progetto, posizionata nella cartella denominata `HelperClasses`. Quando il progetto viene compilato, il codice nel file di `BasePage.cs` viene compilato insieme alle classi code-behind delle pagine ASP.NET nel singolo assembly, `BookReviewsWAP.dll.` ASP.NET dispone di una cartella speciale denominata `App_Code` progettata per conservare i file di classe per i progetti di siti Web. Il codice nella cartella `App_Code` viene compilato automaticamente e pertanto non deve essere utilizzato con i progetti di applicazione Web. Al contrario, è necessario inserire i file di classe dell'applicazione in una cartella normale denominata `HelperClasses`, o `Classes`, o un valore simile. In alternativa, è possibile inserire i file di classe in un progetto libreria di classi distinto.

Oltre a copiare i file di markup correlati a ASP.NET e l'assembly nella cartella `Bin`, è anche necessario copiare i file di supporto lato client, ovvero le immagini e i file CSS, nonché gli altri file di supporto lato server, `Web.config` e `Web.sitemap`. Questi file di supporto lato client e server devono essere copiati nell'ambiente di produzione, indipendentemente dal fatto che si usi la compilazione esplicita o automatica.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinazione dei file da distribuire per i file di progetto del sito Web

Il modello di progetto di sito Web supporta la compilazione automatica, una funzionalità non disponibile quando si usa il modello di progetto di applicazione Web. Con la compilazione esplicita è necessario compilare il codice sorgente del progetto in un assembly e copiare tale assembly nell'ambiente di produzione. D'altra parte, con la compilazione automatica è sufficiente copiare il codice sorgente nell'ambiente di produzione e viene compilato dal runtime su richiesta in base alle esigenze.

L'opzione di menu Compila in Visual Studio è presente nei progetti di applicazione Web e nei progetti di siti Web. La compilazione di un progetto di applicazione Web compila il codice sorgente del progetto in un singolo assembly che si trova nella directory `Bin`; la compilazione di un progetto di sito Web controlla la presenza di errori in fase di compilazione, ma non crea assembly. Per distribuire un'applicazione ASP.NET sviluppata usando il modello di progetto di sito Web, è sufficiente copiare i file appropriati nell'ambiente di produzione, ma si consiglia di compilare innanzitutto il progetto per assicurarsi che non siano presenti errori in fase di compilazione.

Nella figura 3 sono illustrati i file che costituiscono il progetto del sito Web revisioni.

 [![Esplora soluzioni elenca i file che comprendono il progetto di sito Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: l'Esplora soluzioni elenca i file che comprendono il progetto di sito Web

La distribuzione di un progetto di sito Web comporta la copia di tutti i file correlati a ASP.NET nell'ambiente di produzione, che include le pagine di markup per le pagine ASP.NET, le pagine master e i controlli utente, insieme ai relativi file di codice. È anche necessario copiare tutti i file di classe, ad esempio BasePage.cs. Si noti che il file `BasePage.cs` si trova nella cartella `App_Code`, ovvero una speciale cartella ASP.NET usata nei progetti di siti Web per i file di classe. Anche la cartella speciale deve essere creata in fase di produzione, in quanto i file di classe nella cartella `App_Code` nell'ambiente di sviluppo devono essere copiati nella cartella `App_Code` in produzione.

Oltre a copiare il markup ASP.NET e i file di codice sorgente, è necessario copiare anche i file di supporto lato client, ovvero le immagini e i file CSS, nonché gli altri file di supporto lato server, `Web.config` e `Web.sitemap`.

> [!NOTE]
> I progetti di siti Web possono inoltre utilizzare la compilazione esplicita. In un'esercitazione successiva verrà esaminato come compilare in modo esplicito un progetto di sito Web.

## <a name="summary"></a>Riepilogo

La distribuzione di un'applicazione ASP.NET comporta la copia dei file necessari dall'ambiente di sviluppo all'ambiente di produzione. Il set preciso di file che devono essere sincronizzati dipende dal fatto che il codice dell'applicazione ASP.NET venga compilato in modo esplicito o automatico. La strategia di compilazione utilizzata è influenzata dal fatto che Visual Studio sia configurato per gestire l'applicazione ASP.NET usando il modello di progetto di applicazione Web o il modello di progetto di sito Web.

Il modello di progetto applicazione Web utilizza la compilazione esplicita e compila il codice del progetto in un singolo assembly nella cartella `Bin`. Quando si distribuisce l'applicazione, la parte di markup delle pagine ASP.NET e il contenuto della cartella `Bin` devono essere inseriti nell'ambiente di produzione; il codice sorgente nell'applicazione: i file di codice e le classi code-behind, ad esempio, non devono essere copiati nell'ambiente di produzione.

Per impostazione predefinita, il modello di progetto di sito Web utilizza la compilazione automatica, sebbene sia possibile compilare in modo esplicito un progetto di sito Web, come si vedrà nelle esercitazioni future. Per la distribuzione di un'applicazione ASP.NET che usa la compilazione automatica è necessario che la parte di markup *e* il codice sorgente debbano essere copiati nell'ambiente di produzione. Il codice viene compilato automaticamente nell'ambiente di produzione quando viene richiesto per la prima volta.

Ora che sono stati esaminati i file che devono essere sincronizzati tra gli ambienti di sviluppo e di produzione, è possibile distribuire l'applicazione di revisione del libro a un provider host Web.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Panoramica della compilazione di ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Esame di ASP. Esplorazione del sito di NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introduzione ai progetti di applicazione Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Esercitazioni sulla pagina master](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Condivisione di codice tra pagine](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Uso di una classe di base personalizzata per le classi code-behind delle pagine ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema del progetto di sito Web di Visual Studio 2005: che cos'è e perché è stato fatto?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Procedura dettagliata: conversione di un progetto di sito Web in un progetto di applicazione Web in Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Precedente](asp-net-hosting-options-cs.md)
> [Successivo](deploying-your-site-using-an-ftp-client-cs.md)
