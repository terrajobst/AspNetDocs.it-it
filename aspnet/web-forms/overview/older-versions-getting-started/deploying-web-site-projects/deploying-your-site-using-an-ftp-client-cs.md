---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Distribuzione del sito tramite un client FTP (C#) | Microsoft Docs
author: rick-anderson
description: Il modo più semplice per distribuire un'applicazione ASP.NET consiste nel copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. ...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: a3474650939ee220b3fd712e9f5a6cf3db11db09
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545591"
---
# <a name="deploying-your-site-using-an-ftp-client-c"></a>Distribuzione del sito tramite un client FTP (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Il modo più semplice per distribuire un'applicazione ASP.NET consiste nel copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. In questa esercitazione viene illustrato come utilizzare un client FTP per ottenere i file dal desktop al provider host Web.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stata introdotta una semplice applicazione Web Review ASP.NET, che è costituita da un numero limitato di pagine ASP.NET, una pagina master, una classe di base `Page` personalizzata, un numero di immagini e tre fogli di stile CSS. A questo punto è possibile distribuire l'applicazione a un provider di host Web, a cui l'applicazione sarà accessibile a chiunque disponga di una connessione a Internet.

Dalle discussioni nell'esercitazione relativa alla [*determinazione dei file da distribuire*](determining-what-files-need-to-be-deployed-cs.md) , sappiamo quali file devono essere copiati nel provider host Web. Tenere presente che i file copiati variano a seconda che l'applicazione venga compilata in modo esplicito o automatico. Ma come si ottengono i file dall'ambiente di sviluppo (il desktop) all'ambiente di produzione (il server Web gestito dal provider host Web)? [ **F** Ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) è un protocollo di uso comune per la copia dei file da un computer a un altro su una rete. Un'altra opzione è Estensioni del server di FrontPage (FPSE). Questa esercitazione è incentrata sull'utilizzo del software client FTP autonomo per distribuire i file necessari dall'ambiente di sviluppo all'ambiente di produzione.

> [!NOTE]
> Visual Studio include strumenti per la pubblicazione di siti Web tramite FTP; questi strumenti, oltre a esaminare gli strumenti che usano FPSE, sono trattati nell'esercitazione successiva.

Per copiare i file tramite FTP, è necessario un *client FTP* nell'ambiente di sviluppo. Un client FTP è un'applicazione progettata per copiare i file dal computer in cui è installato in un computer in cui è in esecuzione un *server FTP*. Se il provider dell'host Web supporta i trasferimenti di file tramite FTP, come nella maggior parte dei casi, è presente un server FTP in esecuzione nei server Web. Sono disponibili diverse applicazioni client FTP. Il Web browser può anche raddoppiare come un client FTP. Il mio client FTP preferito e quello che utilizzerò per questa esercitazione è [FileZilla](http://filezilla-project.org/), un client FTP open source gratuito disponibile per Windows, Linux e Mac. Tuttavia, qualsiasi client FTP funzionerà, quindi è possibile usare qualsiasi client con cui si ha maggiore dimestichezza.

Se si sta seguendo la procedura, è necessario creare un account con un provider host Web per poter completare questa esercitazione o versioni successive. Come indicato nell'esercitazione precedente, sono disponibili un Gaggle di aziende provider di servizi di hosting Web con una vasta gamma di prezzi, funzionalità e qualità del servizio. Per questa serie di esercitazioni verrà usato [Discount ASP.NET](http://discountasp.net) come provider di host Web, ma è possibile seguire qualsiasi provider di host Web purché supporti la versione di ASP.NET in cui è stato sviluppato il sito. (Queste esercitazioni sono state create con ASP.NET 3,5). Inoltre, dal momento che i file verranno copiati nel provider host Web tramite FTP in questa esercitazione e in quelli futuri è necessario che il provider host Web supporti l'accesso FTP ai server Web. Praticamente tutti i provider host Web offrono questa funzionalità, ma è necessario verificarne il doppio prima di iscriversi.

## <a name="deploying-the-book-review-web-application-project"></a>Distribuzione del progetto di applicazione Web Book Review

Tenere presente che sono disponibili due versioni dell'applicazione Web di revisione del libro: una implementata utilizzando il modello di progetto di applicazione Web (BookReviewsWAP) e l'altra utilizzando il modello di progetto di sito Web (BookReviewsWSP). Il tipo di progetto determina se il sito viene compilato automaticamente o in modo esplicito e il modello di compilazione stabilisce quali file devono essere distribuiti. Di conseguenza, si esaminerà la distribuzione dei progetti BookReviewsWAP e BookReviewsWSP separatamente, a partire da BookReviewsWAP. Se non è già stato fatto, è necessario scaricare queste due applicazioni ASP.NET.

Avviare il progetto BookReviewsWAP passando alla cartella `BookReviewsWAP` e facendo doppio clic sul file di `BookReviewsWAP.sln`. Prima di distribuire il progetto, è importante compilarlo per garantire che tutte le modifiche apportate al codice sorgente siano incluse nell'assembly compilato. Per compilare il progetto, andare al menu Compila e scegliere l'opzione di menu Compila BookReviewsWAP. In questo modo il codice sorgente del progetto viene compilato in un singolo assembly, `BookReviewsWAP.dll`, che viene inserito nella cartella `Bin`.

A questo punto è possibile distribuire i file necessari. Avviare il client FTP e connettersi al server Web dal provider host Web. Quando si effettua l'iscrizione con una società di hosting Web, si inviano messaggi di posta elettronica all'utente per informazioni su come connettersi al server FTP, inclusi l'indirizzo del server FTP, oltre a un nome utente e una password.

Copiare i file seguenti dal desktop alla cartella del sito Web radice nel provider dell'host Web. Quando si esegue l'FTP nel server Web del provider host Web è probabile che si trovino nella directory radice del sito Web. Alcuni provider host Web, tuttavia, dispongono di una sottocartella denominata `www` o `wwwroot` che funge da cartella radice per i file del sito Web. Infine, quando si FTPing i file, potrebbe essere necessario creare la struttura di cartelle corrispondente nell'ambiente di produzione, ovvero la cartella `Bin`, la cartella `Fiction`, la cartella `Images` e così via.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Contenuto completo della cartella `Styles`
- Il contenuto completo della cartella `Images` (e della relativa sottocartella `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

La figura 1 Mostra FileZilla dopo che i file necessari sono stati copiati. FileZilla Visualizza i file nel computer locale a sinistra e i file sul computer remoto a destra. Come illustrato nella figura 1, i file di codice sorgente ASP.NET, ad esempio `About.aspx.cs`, si trovano nel computer locale (l'ambiente di sviluppo) ma non sono stati copiati nel provider host Web (ambiente di produzione) perché i file di codice non devono essere distribuiti quando si usa la compilazione esplicita.

> [!NOTE]
> Non si verificano problemi nel caso in cui i file del codice sorgente nel server di produzione vengano ignorati. Per impostazione predefinita, ASP.NET non consente le richieste HTTP ai file di codice sorgente, in modo che anche se i file di codice sorgente sono presenti nel server di produzione non sono accessibili ai visitatori del sito Web. Ovvero, se un utente tenta di visitare `http://www.yoursite.com/Default.aspx.cs` viene restituita una pagina di errore in cui viene spiegato che questi tipi di file `.cs` file non sono consentiti.

[![utilizzare un client FTP per copiare i file necessari dal desktop al server Web nel provider host Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Figura 1**: usare un client FTP per copiare i file necessari dal desktop al server Web nel provider host Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))

Dopo aver distribuito il sito, è necessario eseguire il test del sito. Se è stato acquistato un nome di dominio e le impostazioni DNS sono state configurate correttamente, è possibile visitare il sito immettendo il nome di dominio. In alternativa, il provider dell'host Web deve avere fornito un URL al sito, che avrà un aspetto simile a *AccountName*. *webhostprovider*. com o *webhostprovider*. com/*AccountName*. Ad esempio, l'URL per il mio account su Discount ASP.NET è: `http://httpruntime.web703.discountasp.net`.

Nella figura 2 viene illustrato il sito delle recensioni dei libri distribuiti. Si noti che viene visualizzato su Discount ASP. Server NET, in `http://httpruntime.web703.discountasp.net`. A questo punto nel tempo chiunque disponga di una connessione a Internet potrebbe visualizzare il sito Web. Come ci si aspetterebbe, il sito sembra e si comporta come se fosse stato testato nell'ambiente di sviluppo.

> [!NOTE]
> Se si verifica un errore durante la visualizzazione dell'applicazione, è necessario assicurarsi di avere distribuito il set di file corretto. Controllare quindi il messaggio di errore per verificare se sono presenti indizi relativi al problema. A questo punto, è possibile rivolgersi al supporto tecnico della società host Web o pubblicare una domanda nel forum appropriato nei [Forum di ASP.NET](https://forums.asp.net/).

[![il sito di revisione del libro è ora accessibile a chiunque disponga di una connessione Internet](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Figura 2**: il sito di revisione del libro è ora accessibile a chiunque disponga di una connessione Internet ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Distribuzione del progetto di sito Web di revisione del libro

Quando si distribuisce un'applicazione ASP.NET che usa la compilazione automatica, ad esempio il progetto sito Web BookReviewsWSP, non è presente alcun assembly compilato nella cartella `Bin`. Di conseguenza, i file di codice sorgente dell'applicazione Web devono essere distribuiti nell'ambiente di produzione. Esaminiamo questo processo.

Come per il progetto di applicazione Web, è consigliabile innanzitutto compilare l'applicazione prima di distribuirla. Durante la compilazione di un progetto di sito Web non viene creato un assembly, viene verificata la presenza di eventuali errori in fase di compilazione nella pagina. Meglio trovare questi errori adesso anziché avere un visitatore del sito per individuarli.

Dopo aver compilato correttamente il progetto, usare il client FTP per copiare i file seguenti nella cartella del sito Web radice nel provider host Web. Potrebbe essere necessario creare la struttura di cartelle corrispondente nell'ambiente di produzione.

> [!NOTE]
> Se è già stato distribuito il progetto BookReviewsWAP ma si vuole ancora provare a distribuire il progetto BookReviewsWSP, eliminare innanzitutto tutti i file sul server Web caricati durante la distribuzione di BookReviewsWAP e quindi distribuire i file per BookReviewsWSP.

- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Contenuto completo della cartella `Styles`
- Il contenuto completo della cartella `Images` (e della relativa sottocartella `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

La figura 3 Mostra FileZilla dopo aver copiato i file necessari. Come si può notare, i file di codice sorgente ASP.NET, ad esempio `About.aspx.cs`, sono presenti sia nel computer locale (ambiente di sviluppo) che nel provider host Web (ambiente di produzione), perché i file di codice devono essere distribuiti quando si usa la compilazione automatica.

[![utilizzare un client FTP per copiare i file necessari dal desktop al server Web nel provider host Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Figura 3**: usare un client FTP per copiare i file necessari dal desktop al server Web nel provider host Web ([fare clic per visualizzare l'immagine con dimensioni complete](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))

L'esperienza utente non è interessata dal modello di compilazione dell'applicazione. Le stesse pagine ASP.NET sono accessibili e sembrano comportarsi allo stesso tempo, indipendentemente dal fatto che il sito Web sia stato creato utilizzando il modello di progetto di applicazione Web o il modello di progetto di sito Web.

### <a name="updating-a-web-application-on-production"></a>Aggiornamento di un'applicazione Web nell'ambiente di produzione

Lo sviluppo e la distribuzione di applicazioni Web non sono un processo una volta. Ad esempio, durante la creazione del sito Web di revisione del libro ho compilato le varie pagine e ho scritto il codice associato nel mio personal computer (l'ambiente di sviluppo). Dopo aver raggiunto un determinato stato stabile, ho distribuito l'applicazione in modo che altri utenti potessero visitare il sito e leggere le mie recensioni. Ma la distribuzione non contrassegna la fine dello sviluppo in questo sito. Posso aggiungere altre recensioni di libri o implementare nuove funzionalità, ad esempio consentire ai miei visitatori di valutare i libri o lasciare i propri commenti. Tali miglioramenti verranno sviluppati nell'ambiente di sviluppo e, al termine, dovranno essere distribuiti. Lo sviluppo e la distribuzione sono pertanto ciclici. Si sviluppa un'applicazione e la si distribuisce. Mentre il sito è attivo e in produzione, vengono aggiunte nuove funzionalità e i bug sono corretti nel tempo, per cui è necessario ridistribuire l'applicazione. E così via.

Come si può immaginare, quando si ridistribuisce un'applicazione Web è necessario copiare solo i file nuovi e modificati. Non è necessario ridistribuire le pagine non modificate, i file di supporto lato server o lato client (anche se non vi è alcun danno a questo scopo).

> [!NOTE]
> Un aspetto da tenere presente quando si usa la compilazione esplicita è che ogni volta che si aggiunge una nuova pagina ASP.NET al progetto o si apportano modifiche correlate al codice, è necessario ricompilare il progetto, che aggiorna l'assembly nella cartella `Bin`. Di conseguenza, è necessario copiare l'assembly aggiornato in produzione durante l'aggiornamento di un'applicazione Web in produzione, insieme ad altri contenuti nuovi e aggiornati.

È inoltre importante comprendere che qualsiasi modifica apportata al `Web.config` o ai file nella directory `Bin` arresta e riavvia il pool di applicazioni del sito Web. Se lo stato della sessione viene archiviato usando la modalità di `InProc` (impostazione predefinita), i visitatori del sito perderanno lo stato della sessione ogni volta che vengono modificati i file di chiave. Per evitare questo problema, provare a archiviare la sessione usando le modalità `StateServer` o `SQLServer`. Per ulteriori informazioni su questo argomento leggere le [modalità di stato sessione](https://msdn.microsoft.com/library/ms178586.aspx).

Infine, tenere presente che la ridistribuzione di un'applicazione può richiedere da alcuni secondi a diversi minuti, a seconda del numero e delle dimensioni dei file che devono essere copiati nell'ambiente di produzione. Durante questo periodo, gli utenti che visitano il sito potrebbero riscontrare errori o comportamenti dispari. È possibile disattivare l'intera applicazione aggiungendo una pagina denominata `App_Offline.htm` alla directory radice dell'applicazione, che spiega agli utenti che il sito è inattivo per la manutenzione (o qualsiasi altra cosa) e verrà eseguito il backup a breve. Quando è presente il file `App_Offline.htm`, il runtime di ASP.NET reindirizza tutte le richieste in ingresso a tale pagina.

## <a name="summary"></a>Riepilogo

La distribuzione di un'applicazione Web comporta la copia dei file necessari dall'ambiente di sviluppo all'ambiente di produzione. Il mezzo più comune con cui i file vengono trasferiti in una rete è il File Transfer Protocol (FTP) e la maggior parte dei provider host Web supporta l'accesso FTP ai server Web. In questa esercitazione è stato illustrato come utilizzare un client FTP per distribuire i file necessari sul server Web. Una volta distribuito, il sito Web può essere visitato da chiunque disponga di una connessione a Internet.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [App\_offline. htm e aggirare la funzionalità "errori intuitivi di Internet Explorer"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modalità stato sessione](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Precedente](determining-what-files-need-to-be-deployed-cs.md)
> [Successivo](deploying-your-site-using-visual-studio-cs.md)
