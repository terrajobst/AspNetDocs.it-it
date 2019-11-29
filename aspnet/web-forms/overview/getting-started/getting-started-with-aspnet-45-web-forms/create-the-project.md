---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Creare il progetto | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576060"
---
# <a name="create-the-project"></a>Creare il progetto

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

In questa esercitazione verrà creato, esaminato ed eseguito il progetto predefinito in Visual Studio, che consente di acquisire familiarità con le funzionalità di ASP.NET. Si esaminerà anche l'ambiente di Visual Studio.

## <a name="what-youll-learn"></a>Cosa si apprenderà:

- Creazione di un nuovo progetto Web Form.
- Struttura di file del progetto Web Form.
- Come eseguire il progetto in Visual Studio.
- Le diverse funzionalità dell'applicazione Web Forms predefinita.
- Alcune nozioni di base su come usare l'ambiente di Visual Studio.

## <a name="creating-the-project"></a>Creazione del progetto

1. Apri Visual Studio.
2. Scegliere **nuovo progetto** dal menu **file** in Visual Studio. 

    ![Crea la voce di menu progetto-nuovo progetto](create-the-project/_static/image1.png)
3. Selezionare i **modelli** -&gt; **Visual C#**  -&gt; gruppo di modelli **Web** a sinistra.
4. Scegliere il modello **applicazione Web ASP.NET** nella colonna centrale.  
 Questa serie di esercitazioni USA .NET Framework 4.5.2.
5. Assegnare al progetto il nome *WingtipToys* e scegliere il pulsante **OK** . 

    ![Creazione della finestra di dialogo progetto-nuovo progetto](create-the-project/_static/image2.png)

    > [!NOTE]
    > Il nome del progetto in questa serie di esercitazioni è **WingtipToys**. Si consiglia di usare questo nome di progetto *esatto* in modo che il codice fornito in tutta la serie di esercitazioni funzioni come previsto.

6. Fare clic sul pulsante **Modifica autenticazione** . Selezionare **singoli account utente** e fare clic sul pulsante **OK** .

7. Selezionare il modello **Web Form** e fare clic sul pulsante **OK** .

    ![Creare il progetto-nuovo modello di progetto](create-the-project/_static/image3.png)

La creazione del progetto potrebbe richiedere un po' di tempo. Quando è pronto, aprire la pagina **default. aspx** .

![Creare il progetto-nuovo modello di progetto](create-the-project/_static/image4.png)

È possibile passare dalla visualizzazione **progettazione** alla visualizzazione **origine** e viceversa selezionando un'opzione nella parte inferiore della finestra centrale. La visualizzazione **progettazione** consente di visualizzare le pagine Web, le pagine master, le pagine di contenuto, le pagine HTML e i controlli utente di ASP.NET utilizzando una visualizzazione quasi WYSIWYG. Visualizzazione **origine** consente di visualizzare il markup HTML per la pagina Web che è possibile modificare.

> [!TIP] 
> 
> **Informazioni sui Framework di ASP.NET**
> 
> Con Web Form ASP.NET è possibile creare siti Web dinamici utilizzando un modello guidato dagli eventi semplice e intuitivo, basato sul trascinamento della selezione. Questo modello di programmazione offre agli sviluppatori un'area di progettazione e numerosi controlli e componenti per creare rapidamente siti Web basati su interfaccia utente potenti e di grande effetto, con capacità di accesso ai dati. Il Toy Store di Wingtip si basa su Web Form ASP.NET, ma molti dei concetti illustrati in questa serie di esercitazioni sono applicabili a tutti i ASP.NET.
> 
> ASP.NET offre quattro Framework di sviluppo principali:
> 
> - [Web Form ASP.NET](../../../index.md)  
>  Il framework Web Forms è destinato agli sviluppatori che preferiscono la programmazione dichiarativa e basata sul controllo, ad esempio Microsoft Windows Forms (WinForms) e WPF/XAML/Silverlight. Offre un modello di sviluppo basato su progettazione WYSIWYG, quindi è popolare per gli sviluppatori che cercano un ambiente di sviluppo di applicazioni rapide (RAD) per lo sviluppo Web. Se non si ha familiarità con la programmazione Web e si ha familiarità con gli strumenti di sviluppo client Microsoft RAD tradizionali (ad C#esempio, per Visual Basic e visuale), è possibile creare rapidamente un'applicazione Web senza avere esperienza in HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC è destinato agli sviluppatori interessati a modelli e principi come lo sviluppo basato su test, la separazione dei problemi, l'inversione del controllo (IoC) e l'inserimento delle dipendenze (DI). Questo Framework incoraggia la separazione del livello di logica di business di un'applicazione Web dal livello di presentazione.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  Pagine Web ASP.NET è destinato agli sviluppatori che desiderano una semplice storia di sviluppo Web, lungo le linee di PHP. Nel modello Web Pages è possibile creare pagine HTML, quindi aggiungere codice basato su server alla pagina per controllare dinamicamente il rendering del markup. Web Pages è appositamente progettato per essere un framework leggero ed è il punto di ingresso più semplice in ASP.NET per le persone che conoscono il codice HTML, ma che potrebbero non avere un'ampia esperienza di programmazione, ad esempio studenti o Hobby. È anche un modo efficace per gli sviluppatori Web che conoscono PHP o Framework simili per iniziare a usare ASP.NET.
> - [Applicazione a pagina singola ASP.NET](../../../../single-page-application/index.md)  
>  L'applicazione a pagina singola ASP.NET (SPA) consente di creare applicazioni che includono interazioni significative sul lato client con HTML 5, CSS 3 e JavaScript. L'aggiornamento di ASP.NET and Web Tools 2012,2 fornisce un nuovo modello per la creazione di applicazioni a pagina singola con Knockout. js e API Web ASP.NET. Oltre al nuovo modello di applicazione a singola pagina, sono disponibili per il download anche i nuovi modelli SPA creati dalla community.
> 
> Oltre ai quattro Framework principali per lo sviluppo, ASP.NET offre anche tecnologie aggiuntive importanti da conoscere e che non sono incluse in questa serie di esercitazioni:
> 
> - [API Web ASP.NET](../../../../web-api/index.md) : Framework per la creazione di servizi HTTP che raggiungono un'ampia gamma di client, inclusi browser e dispositivi mobili.
> - [ASP.NET SignalR](../../../../signalr/index.md) : una libreria che rende più semplice lo sviluppo di funzionalità Web in tempo reale.

### <a name="reviewing-the-project"></a>Revisione del progetto

In Visual Studio la finestra di **Esplora soluzioni** consente di gestire i file per il progetto. Verranno ora esaminate le cartelle che sono state aggiunte all'applicazione in **Esplora soluzioni**. Il modello di applicazione Web aggiunge una struttura di cartelle di base:

![Creare il progetto Esplora soluzioni](create-the-project/_static/image5.png)

Visual Studio crea alcuni file e cartelle iniziali per il progetto. I primi file che verranno elaborati più avanti in questa esercitazione sono i seguenti:

| **File** | **Scopo** |
| --- | --- |
| *Default. aspx* | In genere, la prima pagina viene visualizzata quando l'applicazione viene eseguita in un browser. |
| *Site. master* | Pagina che consente di creare un layout coerente e di utilizzare il comportamento standard per le pagine dell'applicazione. |
| *Global. asax* | File facoltativo che contiene il codice per rispondere agli eventi a livello di applicazione e a livello di sessione generati da ASP.NET o da moduli HTTP. |
| *Web. config* | Dati di configurazione per un'applicazione. |

### <a name="running-the-default-web-application"></a>Esecuzione dell'applicazione Web predefinita

L'applicazione Web predefinita offre un'esperienza avanzata basata sulle funzionalità e il supporto predefiniti. Senza apportare modifiche al progetto Web form predefinito, l'applicazione è pronta per essere eseguita nel Web browser locale.

1. Premere il tasto ***F5*** in Visual Studio.   
 L'applicazione viene compilata e visualizzata nel Web browser.  

    ![Creare il progetto-pagina predefinita](create-the-project/_static/image6.png)
2. Dopo aver completato la verifica dell'applicazione in esecuzione, chiudere la finestra del browser.

In questa applicazione Web predefinita sono disponibili tre pagine principali: *default. aspx* (Home), *About. aspx*e *Contact. aspx*. Ognuna di queste pagine può essere raggiunta dalla barra di spostamento superiore. Nella cartella account sono inoltre presenti due pagine aggiuntive, la pagina Register. aspx e la pagina login. aspx. Queste due pagine consentono di usare le funzionalità di appartenenza di ASP.NET per creare, archiviare e convalidare le credenziali utente.

## <a name="aspnet-web-forms-background"></a>Sfondo Web Form ASP.NET

I Web Form ASP.NET sono pagine basate su Microsoft ASP.NET tecnologia, in cui il codice in esecuzione nel server genera in modo dinamico l'output della pagina Web nel browser o nel dispositivo client. Una pagina Web Form ASP.NET esegue automaticamente il rendering del codice HTML conforme al browser corretto per le funzionalità quali stili, layout e così via. I Web Form sono compatibili con qualsiasi linguaggio supportato dalla Common Language Runtime .NET, ad esempio Microsoft Visual Basic e Microsoft Visual C#. Inoltre, i Web Form sono basati sul [Framework Microsoft .NET](https://msdn.microsoft.com/vstudio/aa496123), che offre vantaggi quali un ambiente gestito, l'indipendenza dai tipi e l'ereditarietà.

Quando viene eseguita una pagina Web Form ASP.NET, la pagina passa attraverso un ciclo di vita in cui viene eseguita una serie di passaggi di elaborazione. Questi passaggi includono l'inizializzazione, l'istanza dei controlli, il ripristino e la gestione dello stato, il codice del gestore eventi in esecuzione e il rendering. Man mano che si acquisisce familiarità con la potenza di ASP.NET Web Form, è importante comprendere il [ciclo di vita della pagina ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) in modo da poter scrivere il codice nella fase del ciclo di vita appropriata per l'effetto desiderato.

Quando un server Web riceve una richiesta per una pagina, trova la pagina, la elabora, la invia al browser e quindi Elimina tutte le informazioni sulla pagina. Se l'utente richiede nuovamente la stessa pagina, il server ripete l'intera sequenza, rielaborando la pagina da zero. In altre parole, un server non dispone di memoria delle pagine elaborate. le pagine sono senza stato. Il Framework della pagina ASP.NET gestisce automaticamente l'attività di gestione dello stato della pagina e dei relativi controlli e fornisce modi espliciti per mantenere lo stato delle informazioni specifiche dell'applicazione.

> [!TIP] 
> 
> **Funzionalità dell'applicazione Web nel modello di applicazione Web Form**
> 
> Il modello di applicazione Web Form ASP.NET offre un set completo di funzionalità predefinite. Non solo fornisce una pagina *Home. aspx* , una pagina *About.* aspx, una pagina *Contact. aspx* , ma include anche la funzionalità di appartenenza che registra gli utenti e salva le credenziali in modo che possano accedere al sito Web. In questa panoramica vengono fornite ulteriori informazioni su alcune delle funzionalità contenute nel modello di applicazione Web Form ASP.NET e sul relativo utilizzo nell'applicazione Wingtip Toys.
> 
> **Appartenenza**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity archivia le credenziali degli utenti in un database creato dall'applicazione. Quando gli utenti accedono, l'applicazione convalida le proprie credenziali leggendo il database. La cartella *account* del progetto contiene i file che implementano le varie parti di appartenenza: registrazione, accesso, modifica di una password e autorizzazione dell'accesso. ASP.NET Web Forms supporta inoltre OAuth e OpenID. Questi miglioramenti dell'autenticazione consentono agli utenti di accedere al sito usando le credenziali esistenti, da account come Facebook, Twitter, Windows Live e Google.
> 
> ![Creare il Esplora soluzioni di progetto (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Per impostazione predefinita, il modello crea un database di appartenenza utilizzando un nome di database predefinito in un'istanza di SQL Server Express database locale, il server di database di sviluppo che include Visual Studio Express 2013 per il Web.
> 
> **SQL Server Express database locale**
> 
> [SQL Server Express database locale](https://technet.microsoft.com/library/hh510202.aspx) è una versione leggera di SQL Server con numerose funzionalità di programmabilità di un database SQL Server. SQL Server Express database locale viene eseguito in modalità utente ed è dotato di un'installazione veloce e con configurazione zero con un breve elenco di prerequisiti di installazione. In Microsoft SQL Server è possibile spostare qualsiasi database o codice Transact-SQL da SQL Server Express database locale a SQL Server e SQL Azure senza alcuna procedura di aggiornamento. Pertanto, SQL Server Express database locale può essere utilizzato come ambiente di sviluppo per le applicazioni destinate a tutte le edizioni di SQL Server. SQL Server Express database locale Abilita funzionalità quali stored procedure, funzioni definite dall'utente e aggregazioni, .NET Framework integrazione, tipi spaziali e altri non disponibili in SQL Server Compact.
> 
> **Pagine master**
> 
> Una [pagina master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definisce un aspetto e un comportamento coerenti per tutte le pagine dell'applicazione. Il layout della pagina master si unisce al contenuto di una singola pagina di contenuto per produrre la pagina finale visualizzata dall'utente. Nell'applicazione Wingtip Toys è possibile modificare la pagina master *site. master* in modo che tutte le pagine del sito Web Wingtip Toys condividano lo stesso logo distintivo e la stessa barra di spostamento.
> 
> **HTML5**
> 
> Il modello di applicazione Web Form ASP.NET supporta [HTML5](http://www.w3schools.com/html/html5_intro.asp), che è la versione più recente del linguaggio di markup HTML. HTML5 supporta nuovi elementi e funzionalità che semplificano la creazione di siti Web.
> 
> **Modernizr**
> 
> Per i browser che non supportano HTML5, è possibile usare [modernizzatore](http://www.modernizr.com/). Modernizzator è una libreria JavaScript open source in grado di rilevare se un browser supporta le funzionalità HTML5 e di abilitarle in caso contrario. Nel modello di applicazione Web Form ASP.NET, modernizzatore viene installato come pacchetto NuGet.
> 
> **Bootstrap**
> 
> I modelli di progetto Visual Studio 2013 usano [bootstrap](http://getbootstrap.com/), un layout e un Framework di tema creati da Twitter. Bootstrap USA CSS3 per fornire una progettazione reattiva, il che significa che i layout possono adattarsi dinamicamente alle diverse dimensioni della finestra del browser. È anche possibile usare la funzionalità di tema di bootstrap per applicare facilmente una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include bootstrap come pacchetto NuGet.
> 
> **Pacchetti NuGet**
> 
> Il modello di applicazione Web Forms di ASP.NET include un set di pacchetti [NuGet](http://www.nuget.org/) . Questi pacchetti forniscono funzionalità con componenti sotto forma di librerie e strumenti open source. È disponibile un'ampia gamma di pacchetti che consentono di creare e testare le applicazioni. Visual Studio semplifica l'aggiunta, la rimozione e l'aggiornamento dei pacchetti NuGet. Gli sviluppatori possono anche creare e aggiungere pacchetti a NuGet.
> 
> ![Creare la finestra di dialogo progetto-NuGet](create-the-project/_static/image8.png)
> 
> Quando si installa un pacchetto, NuGet copia i file nella soluzione e apporta automaticamente tutte le modifiche necessarie, ad esempio l'aggiunta di riferimenti e la modifica della configurazione associata all'applicazione Web. Se si decide di rimuovere la libreria, NuGet rimuove i file e inverte le modifiche apportate nel progetto in modo che non rimanga confusione. NuGet è disponibile dal menu **strumenti** in Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) è una libreria JavaScript rapida e concisa che semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e le interazioni Ajax per lo sviluppo Web rapido. La libreria JavaScript jQuery è inclusa nel modello di applicazione Web Form ASP.NET come pacchetto NuGet.
> 
> **Convalida non intrusiva**
> 
> I controlli validator incorporati sono stati configurati per l'utilizzo di JavaScript non intrusivo per la logica di convalida lato client. Questa operazione riduce significativamente la quantità di codice JavaScript inline nel markup della pagina e riduce le dimensioni complessive della pagina. La convalida non intrusiva viene aggiunta a livello globale al modello di applicazione Web Form ASP.NET in base all'impostazione nell'elemento &lt;appSettings&gt; del file *Web. config* nella radice dell'applicazione.
> 
> **Entity Framework Code First**
> 
> Oltre alle funzionalità del modello di applicazione Web Form ASP.NET, l'applicazione Wingtip Toys USA [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), ovvero una libreria NuGet che consente lo sviluppo incentrato sul codice quando si lavora con i dati. In pratica, crea la parte del database dell'applicazione in base al codice scritto dall'utente. Utilizzando la Entity Framework, è possibile recuperare e modificare i dati come oggetti fortemente tipizzati. Ciò consente di concentrarsi sulla logica di business nell'applicazione anziché sui dettagli relativi all'accesso ai dati.
> 
> Per ulteriori informazioni sulle librerie e sui pacchetti installati inclusi nel modello Web Form ASP.NET, vedere l'elenco dei pacchetti NuGet installati. A tale scopo, in Visual Studio creare un nuovo progetto Web Form selezionare **strumenti** > **gestione pacchetti NuGet** > **Gestisci pacchetti NuGet per la soluzione**e selezionare **pacchetti installati** nella finestra di dialogo **Gestisci pacchetti NuGet** .

### <a name="touring-visual-studio"></a>Touring di Visual Studio

Le finestre primarie in Visual Studio includono **il Esplora soluzioni**, il **Esplora server** (**Esplora database** in Express), **la finestra Proprietà**, la **casella degli**strumenti, la **barra degli strumenti**e la finestra del **documento**.

![Creare la finestra di dialogo progetto-NuGet](create-the-project/_static/image9.png)

Per ulteriori informazioni su Visual Studio, vedere [Guida visiva a Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato creato, esaminato ed eseguito l'applicazione Web Forms predefinita. Sono state esaminate le diverse funzionalità dell'applicazione Web Forms predefinita e sono state apprese alcune nozioni di base sull'uso dell'ambiente Visual Studio. Nelle esercitazioni seguenti verrà creato il livello di accesso ai dati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Scelta del modello di programmazione appropriato](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Progetti di applicazioni Web rispetto ai progetti di siti web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Panoramica delle pagine Web Form ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Precedente](introduction-and-overview.md)
> [Successivo](create_the_data_access_layer.md)
