---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Distribuzione del sito tramite Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti sono disponibili in questa esercitazione.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c71e36a8a434947882cc767cd2f903ff6e8d422
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587213"
---
# <a name="deploying-your-site-using-visual-studio-vb"></a>Distribuzione del sito tramite Visual Studio (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti sono disponibili in questa esercitazione.

## <a name="introduction"></a>Introduzione

L'esercitazione precedente ha esaminato come distribuire una semplice applicazione Web ASP.NET in un provider di host Web. In particolare, l'esercitazione ha illustrato come usare un client FTP come FileZilla per trasferire i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Visual Studio offre anche strumenti predefiniti per facilitare la distribuzione a un provider di host Web. In questa esercitazione vengono esaminati due di questi strumenti: lo strumento Copia sito Web, in cui è possibile spostare i file da e verso un server Web remoto utilizzando FTP o Estensioni del server di FrontPage; e lo strumento di pubblicazione, che copia l'intero sito Web in un percorso specificato.

> [!NOTE]
> Altri strumenti relativi alla distribuzione offerti da Visual Studio includono i [progetti di installazione Web](https://msdn.microsoft.com/library/wx3b589t.aspx) e i [progetti di distribuzione Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . I progetti di installazione Web impacchettano il contenuto e le informazioni di configurazione di un sito Web in un unico file MSI. Questa opzione è particolarmente utile per i siti Web distribuiti in una rete Intranet o per le aziende che vendono un'applicazione Web preconfezionata che i clienti installano nei propri server Web. Il componente aggiuntivo dei progetti di distribuzione Web è un componente aggiuntivo di Visual Studio che facilita la definizione delle differenze di configurazione tra le compilazioni per ambienti di sviluppo e ambienti di produzione. I progetti di installazione Web non vengono discussi in questa serie di esercitazioni. I progetti di distribuzione Web sono riepilogati nelle [*differenze di configurazione comuni tra l'esercitazione sviluppo e produzione*](common-configuration-differences-between-development-and-production-vb.md) .

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Distribuzione del sito tramite lo strumento Copia sito Web

Lo strumento Copia sito Web di Visual Studio è analogo alla funzionalità di un client FTP autonomo. In breve, lo strumento Copia sito Web consente di connettersi a un sito Web remoto tramite FTP o Estensioni del server di FrontPage. Analogamente all'interfaccia utente di FileZilla, l'interfaccia utente di copia sito Web è costituita da due riquadri: il riquadro sinistro elenca i file locali mentre nel riquadro destro sono elencati i file nel server di destinazione.

> [!NOTE]
> Lo strumento Copia sito Web è disponibile solo per i progetti di siti Web. Visual Studio offre questo strumento quando si lavora con un progetto di applicazione Web.

Verrà ora esaminato l'utilizzo dello strumento Copia sito Web per pubblicare l'applicazione di revisione del libro nell'ambiente di produzione. Poiché lo strumento Copia sito Web funziona solo con i progetti che utilizzano il modello di progetto di sito Web, è possibile esaminare solo utilizzando questo strumento con il progetto BookReviewsWSP. Aprire il progetto.

Avviare il progetto copia strumento sito Web facendo clic sull'icona Copia sito Web nella Esplora soluzioni (questa icona è racchiusa tra i cerchi nella figura 1); in alternativa, è possibile selezionare l'opzione Copia sito Web dal menu sito Web. Entrambi gli approcci avviano l'interfaccia utente copia sito Web illustrata nella figura 1. solo il riquadro sinistro della figura 1 viene popolato perché è ancora necessario connettersi a un server remoto.

[![l'interfaccia utente dello strumento Copia sito Web è divisa in due riquadri](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figura 1**: l'interfaccia utente dello strumento Copia sito Web è divisa in due riquadri ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image3.png))

Per distribuire il sito, è necessario prima connettersi al provider dell'host Web. Fare clic sul pulsante Connetti nella parte superiore dell'interfaccia utente copia sito Web. Verrà visualizzata la finestra di dialogo Apri sito Web illustrato nella figura 2.

È possibile connettersi al sito Web di destinazione selezionando una delle quattro opzioni da sinistra:

- **File System** : selezionare questa operazione per distribuire il sito in una cartella o in una condivisione di rete accessibile dal computer.
- **IIS locale** : usare questa opzione per distribuire il sito nel server Web IIS installato nel computer.
- **Sito FTP** : connettersi a un sito Web remoto mediante FTP.
- **Sito remoto** : connettersi a un sito Web remoto usando estensioni del server di FrontPage.

La maggior parte dei provider host Web supporta FTP, ma meno il supporto per l'estensione del server di FrontPage. Per questo motivo, è stata selezionata l'opzione sito FTP, quindi sono state immesse le informazioni di connessione, come illustrato nella figura 2.

[![specificare il sito Web di destinazione](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figura 2**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image6.png))

Dopo la connessione, lo strumento Copia sito Web carica i file nel sito remoto nel riquadro di destra e indica lo stato di ogni file: nuovo, eliminato, modificato o non modificato. È possibile copiare un file dal sito locale al sito remoto, o viceversa.

Aggiungere una nuova pagina al progetto BookReviewsWSP e distribuirla in modo da poter visualizzare lo strumento Copia sito Web in azione. Creare una nuova pagina ASP.NET in Visual Studio nella directory radice denominata `Privacy.aspx`. Fare in modo che la pagina usi la pagina master `Site.master` e aggiungere l'informativa sulla privacy del sito a questa pagina. La figura 3 Mostra Visual Studio dopo la creazione di questa pagina.

[![aggiungere una nuova pagina denominata &lt;code&gt;privacy. aspx&lt;/codice&gt; alla cartella radice del sito Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figura 3**: aggiungere una nuova pagina denominata `Privacy.aspx` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image9.png))

Tornare quindi all'interfaccia utente copia sito Web. Come illustrato nella figura 4, il riquadro sinistro include ora i nuovi file: `Policy.aspx` e `Policy.aspx.vb`. I file sono contrassegnati con un'icona a freccia e uno stato nuovo, a indicare che sono presenti nel sito locale ma non nel sito remoto.

[![lo strumento Copia sito Web include il nuovo codice &lt;&gt;privacy. aspx&lt;pagina&gt;/codice nel riquadro a sinistra](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figura 4**: lo strumento Copia sito Web include la nuova pagina `Privacy.aspx` nel riquadro sinistro ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image12.png))

Per distribuire i nuovi file, selezionarli e quindi fare clic sull'icona della freccia per trasferirli al sito remoto. Al termine del trasferimento, il `Policy.aspx` e i file `Policy.aspx.vb` sono presenti nei siti locali e remoti con lo stato non modificato.

Oltre a elencare nuovi file, lo strumento Copia sito Web evidenzia tutti i file che differiscono tra i siti locali e remoti. Per verificarne il funzionamento, tornare alla pagina `Privacy.aspx` e aggiungere altre parole all'informativa sulla privacy. Salvare la pagina e tornare allo strumento Copia sito Web. Come illustrato nella figura 5, il `Privacy.aspx` pagina nel riquadro sinistro presenta lo stato modificato indicante che non è sincronizzato con il sito remoto.

[![lo strumento Copia sito Web indica che la pagina del codice &lt;&gt;privacy. aspx&lt;/codice&gt; è stata modificata](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figura 5**: lo strumento Copia sito Web indica che la pagina `Privacy.aspx` è stata modificata ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image15.png))

Lo strumento Copia sito Web indica inoltre se un file è stato eliminato dall'ultima operazione di copia. Eliminare il `Privacy.aspx` dal progetto locale e aggiornare lo strumento Copia sito Web. I file `Privacy.aspx` e `Privacy.aspx.vb` rimangono elencati nel riquadro sinistro, ma hanno uno stato eliminato che indica che sono stati rimossi dall'ultima operazione di copia.

## <a name="publishing-a-web-application"></a>Pubblicazione di un'applicazione Web

Un altro modo per distribuire l'applicazione Web dall'interno di Visual Studio consiste nell'usare l'opzione pubblica, accessibile tramite il menu Compila. L'opzione pubblica compila in modo esplicito l'applicazione, quindi copia tutti i file necessari fino al sito remoto specificato. Come si vedrà a breve, l'opzione di pubblicazione è più smussata rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di esaminare i file presenti nei siti locali e remoti e consente di caricare o scaricare singoli file in base alle esigenze, l'opzione pubblica distribuisce l'intera applicazione Web.

Oltre a copiare tutti i file necessari nel sito remoto specificato, l'opzione Publish compila anche in modo esplicito l'applicazione. Dato che i progetti di applicazioni Web devono essere compilati in modo esplicito, non dovrebbe sorprendere che l'opzione pubblica sia disponibile per i progetti di applicazioni Web. Un po' sorprendente è che l'opzione pubblica è disponibile anche per i progetti di siti Web. Come indicato nell'esercitazione [*determinazione dei file da distribuire*](determining-what-files-need-to-be-deployed-vb.md) , i progetti di siti Web possono essere compilati in modo esplicito tramite un processo definito *pre-compilazione*. Questa esercitazione è incentrata sull'uso dell'opzione Publish con i progetti di applicazione Web. un'esercitazione futura esplorerà la pre-compilazione e a questo punto si tornerà a esaminare l'utilizzo dell'opzione di pubblicazione con i progetti di siti Web.

> [!NOTE]
> Sebbene l'opzione pubblica sia disponibile in Visual Studio per i progetti di siti Web e per i progetti di applicazioni Web, Visual Web Developer offre solo l'opzione di pubblicazione per i progetti di applicazioni Web.

Si esaminerà ora la distribuzione dell'applicazione Book revisioni utilizzando l'opzione Publish. Per iniziare, aprire BookReviewsWAP (progetto applicazione Web) in Visual Studio. Scegliere Compila progetto BookReviewsWAP dal menu pubblica. Viene visualizzata una finestra di dialogo in cui viene richiesto il percorso di destinazione, tra le altre opzioni di configurazione (vedere la figura 6). Analogamente a quanto avviene con lo strumento Copia sito Web, è possibile immettere un percorso che punta a una cartella locale, a un sito Web locale in IIS, a un sito Web remoto che supporta Estensioni del server di FrontPage o a un indirizzo server FTP. È possibile scegliere se sostituire i file nel server Web remoto con i file distribuiti o eliminare tutto il contenuto nel sito remoto prima della pubblicazione. È inoltre possibile specificare se copiare:

- Solo i file del progetto necessari per eseguire l'applicazione, che omette il codice sorgente e i file correlati al progetto non necessari.
- Tutti i file di progetto, inclusi i file di codice sorgente e i file di progetto di Visual Studio, come il file di soluzione.
- Tutti i file nella cartella del progetto di origine, che copia tutti i file nella cartella del progetto di origine, indipendentemente dal fatto che siano inclusi nel progetto.

È anche possibile caricare il contenuto della cartella `App_Data`.

[![specificare il sito Web di destinazione](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figura 6**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image18.png))

Per l'applicazione Book Review il sito remoto contiene i file distribuiti durante la copia del progetto BookReviewsWSP tramite lo strumento Copia sito Web. Per questo motivo, è necessario che l'opzione di pubblicazione venga avviata eliminando tutto il contenuto esistente. Inoltre, è sufficiente copiare i file necessari piuttosto che ingombrare l'ambiente di produzione con il codice sorgente e i file di progetto non necessari. Dopo aver specificato queste opzioni, fare clic sul pulsante pubblica. Nei prossimi secondi, Visual Studio distribuirà i file necessari nel sito di destinazione, visualizzandone lo stato di avanzamento nella finestra di output.

La figura 7 Mostra i file sul sito FTP dopo il completamento dell'operazione di pubblicazione. Si noti che sono state caricate solo le pagine di markup e i file di supporto del lato client e del server necessari.

[![solo i file necessari sono stati pubblicati nell'ambiente di produzione](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figura 7**: solo i file necessari sono stati pubblicati nell'ambiente di produzione ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-visual-studio-vb/_static/image21.png))

L'opzione Publish è uno strumento meno sfumato rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di ispezionare i file nei siti locali e remoti e di verificarne le differenze, l'opzione pubblica non fornisce tale interfaccia. Inoltre, lo strumento Copia sito Web consente di apportare modifiche individuali, caricare o eliminare singoli file. L'opzione Publish non consente un controllo con granularità fine. viene invece pubblicata l' *intera* applicazione. Questo comportamento presenta vantaggi e svantaggi. Sul lato più, si sa quando si usa l'opzione pubblica non si dimentica di caricare un file importante. Tuttavia, si consideri cosa accade se è stata apportata una piccola modifica a un sito Web di grandi dimensioni, con l'opzione pubblica non è possibile aggiornare la pagina o due che è stata modificata, ma è invece necessario attendere mentre Visual Studio distribuisce l'intero sito.

Non è insolito che ci siano determinati file il cui contenuto è diverso tra gli ambienti di produzione e di sviluppo. Un esempio chiave è il file di configurazione dell'applicazione, `Web.config`. Poiché l'opzione Pubblica copia ciecamente i file dell'applicazione Web, sovrascrive i file di configurazione personalizzati dell'ambiente di produzione con la versione nell'ambiente di sviluppo. L'esercitazione successiva esamina ulteriormente questo argomento e offre suggerimenti per la distribuzione di un'applicazione Web quando tali differenze sono disponibili.

## <a name="summary"></a>Riepilogo

La distribuzione di un sito Web implica la copia dei file necessari dall'ambiente di sviluppo all'ambiente di produzione. L'esercitazione precedente ha illustrato come trasferire file usando un client FTP come FileZilla. Questa esercitazione ha esaminato due strumenti di distribuzione in Visual Studio: lo strumento Copia sito Web e l'opzione pubblica. Lo strumento Copia sito Web è simile a un client FTP in quanto dispone di un'interfaccia a due riquadri elencando i file nel computer locale e un computer remoto specificato che semplifica il caricamento o il download di file tra i due computer. L'opzione Publish è uno strumento più smussato che compila in modo esplicito il progetto e quindi distribuisce l'intera applicazione nella destinazione specificata.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Copia di un sito Web con lo strumento Copia sito Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Procedura: distribuzione di un sito Web mediante lo strumento Copia sito Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (video)
- [Procedura: pubblicare progetti di applicazione Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Procedura: pubblicare siti Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Progetti di installazione e distribuzione in Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-your-site-using-an-ftp-client-vb.md)
> [Successivo](common-configuration-differences-between-development-and-production-vb.md)
