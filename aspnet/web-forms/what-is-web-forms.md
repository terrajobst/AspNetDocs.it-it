---
uid: web-forms/what-is-web-forms
title: Che cosa sono i Web Form | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636577"
---
# <a name="what-is-web-forms"></a>Che cosa sono i Web Form

ASP.NET Web Forms fa parte del Framework applicazione Web ASP.NET ed è incluso in [Visual Studio](https://www.asp.net/downloads). Si tratta di uno dei quattro modelli di programmazione che è possibile usare per creare applicazioni Web ASP.NET, altre sono le applicazioni ASP.NET MVC, Pagine Web ASP.NET e ASP.NET a pagina singola.

I Web Form sono pagine richieste dagli utenti tramite il browser. Queste pagine possono essere scritte utilizzando una combinazione di HTML, script client, controlli server e codice server. Quando gli utenti richiedono una pagina, la pagina viene compilata ed eseguita nel server dal Framework e quindi il Framework genera il markup HTML che il browser è in grado di eseguire. Una pagina Web Form ASP.NET presenta informazioni all'utente in qualsiasi browser o dispositivo client.

Con Visual Studio è possibile creare Web Form ASP.NET. L'ambiente di sviluppo integrato (IDE) di Visual Studio consente di trascinare i controlli server per il layout della pagina Web Form. È quindi possibile impostare facilmente proprietà, metodi ed eventi per i controlli nella pagina o per la pagina stessa. Queste proprietà, i metodi e gli eventi vengono usati per definire il comportamento della pagina Web, l'aspetto e così via. Per scrivere codice server per gestire la logica della pagina, è possibile usare un linguaggio .NET come Visual Basic o C#.

> [!NOTE] 
> 
> La documentazione di ASP.NET e Visual Studio si estende su diverse versioni. Gli argomenti che evidenziano le funzionalità delle versioni precedenti possono essere utili per le attività e gli scenari correnti utilizzando le versioni più recenti.

**ASP.NET Web Forms:** 

- Basato sulla tecnologia Microsoft ASP.NET, in cui il codice in esecuzione nel server genera in modo dinamico l'output della pagina Web nel browser o nel dispositivo client.
- Compatibile con qualsiasi browser o dispositivo mobile. Una pagina Web di ASP.NET esegue automaticamente il rendering del codice HTML conforme al browser corretto per le funzionalità quali stili, layout e così via.
- Compatibile con qualsiasi linguaggio supportato dalla Common Language Runtime .NET, ad esempio Microsoft Visual Basic e Microsoft Visual C#.
- Basato sul Framework Microsoft .NET. Ciò offre tutti i vantaggi del Framework, tra cui un ambiente gestito, l'indipendenza dai tipi e l'ereditarietà.
- Flessibilità perché è possibile aggiungere controlli creati dall'utente e da terze parti.

**Offerta Web Form ASP.NET:** 

- Separazione di HTML e altro codice dell'interfaccia utente dalla logica dell'applicazione.
- Una suite completa di controlli server per le attività comuni, incluso l'accesso ai dati.
- Data binding potenti, grazie al supporto di strumenti eccezionali.
- Supporto per lo scripting sul lato client eseguito nel browser.
- Supporto per un'ampia gamma di altre funzionalità, tra cui routing, sicurezza, prestazioni, internazionalizzazione, test, debug, gestione degli errori e gestione dello stato.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Form consente di superare le esigenze

La programmazione di applicazioni Web presenta problemi che in genere non si verificano durante la programmazione di applicazioni tradizionali basate su client. Tra i problemi sono:

- **Implementazione di un'interfaccia utente Web avanzata** . può essere difficile e noioso progettare e implementare un'interfaccia utente usando le funzionalità HTML di base, specialmente se la pagina presenta un layout complesso, una grande quantità di contenuto dinamico e oggetti interattivi utente completi.
- **Separazione tra client e server** : in un'applicazione Web, il client (browser) e il server sono programmi diversi spesso in esecuzione in computer diversi (e anche in sistemi operativi diversi). Di conseguenza, le due metà dell'applicazione condividono poche informazioni; possono comunicare, ma in genere scambiare solo piccoli blocchi di informazioni semplici.
- **Esecuzione** senza stato: quando un server Web riceve una richiesta per una pagina, trova la pagina, la elabora, la invia al browser e quindi Elimina tutte le informazioni sulla pagina. Se l'utente richiede nuovamente la stessa pagina, il server ripete l'intera sequenza, rielaborando la pagina da zero. In un altro modo, in un server non è disponibile memoria per le pagine elaborate, ovvero la pagina è senza stato. Pertanto, se un'applicazione deve gestire informazioni su una pagina, la sua natura senza stato può diventare un problema.
- **Funzionalità client sconosciute** : in molti casi, le applicazioni Web sono accessibili a molti utenti che usano browser diversi. I browser hanno funzionalità diverse, rendendo difficile la creazione di un'applicazione che verrà eseguita ugualmente correttamente su tutti i browser.
- Le **complicazioni con l'accesso ai dati** : la lettura e la scrittura in un'origine dati nelle applicazioni Web tradizionali possono essere complesse e a elevato utilizzo di risorse.
- **Complicazioni con la scalabilità** : in molti casi le applicazioni Web progettate con i metodi esistenti non soddisfano gli obiettivi di scalabilità dovuti alla mancanza di compatibilità tra i vari componenti dell'applicazione. Si tratta spesso di un punto di errore comune per le applicazioni in un ciclo di crescita molto elevato.

Il raggiungimento di queste difficoltà per le applicazioni Web può richiedere tempi e sforzi sostanziali. ASP.NET Web Forms e il framework ASP.NET affrontano questi problemi nei modi seguenti:

- **Modello a oggetti intuitivo e coerente** : il Framework della pagina ASP.NET presenta un modello a oggetti che consente di considerare i moduli come un'unità, non come parti separate di client e server. In questo modello è possibile programmare la pagina in modo più intuitivo rispetto alle applicazioni Web tradizionali, inclusa la possibilità di impostare le proprietà per gli elementi della pagina e rispondere agli eventi. Inoltre, i controlli server ASP.NET sono un'astrazione dal contenuto fisico di una pagina HTML e dall'interazione diretta tra browser e server. In generale, è possibile utilizzare i controlli server nel modo in cui è possibile utilizzare i controlli in un'applicazione client e non è necessario pensare a come creare il codice HTML per presentare ed elaborare i controlli e il relativo contenuto.
- **Modello di programmazione basato sugli eventi** : ASP.NET Web Forms consente alle applicazioni Web di scrivere i gestori eventi per gli eventi che si verificano sul client o sul server. Il Framework della pagina ASP.NET astrae questo modello in modo che il meccanismo sottostante di acquisizione di un evento sul client, la trasmissione al server e la chiamata del metodo appropriato sia automatico e invisibile all'utente. Il risultato è una struttura di codice chiara e facilmente scritta che supporta lo sviluppo basato su eventi.
- **Gestione dello stato intuitiva** : il Framework della pagina ASP.NET gestisce automaticamente l'attività di gestione dello stato della pagina e dei relativi controlli e fornisce modi espliciti per mantenere lo stato delle informazioni specifiche dell'applicazione. Questa operazione viene eseguita senza utilizzo intensivo delle risorse del server e può essere implementata con o senza inviare cookie al browser.
- **Applicazioni indipendenti dal browser** : il Framework della pagina ASP.NET consente di creare tutta la logica dell'applicazione nel server, eliminando la necessità di scrivere codice in modo esplicito per le differenze nei browser. Tuttavia, consente di sfruttare le funzionalità specifiche del browser scrivendo il codice lato client per offrire prestazioni migliorate e un'esperienza client più ricca.
- **Supporto di .NET Framework Common Language Runtime** : il Framework della pagina ASP.NET è basato sulla .NET Framework, quindi l'intero Framework è disponibile per qualsiasi applicazione ASP.NET. Le applicazioni possono essere scritte in qualsiasi linguaggio compatibile con il Runtime. Inoltre, l'accesso ai dati viene semplificato usando l'infrastruttura di accesso ai dati fornita dal .NET Framework, incluso ADO.NET.
- **.NET Framework prestazioni server scalabili** : il Framework della pagina ASP.NET consente di ridimensionare l'applicazione Web da un computer con un singolo processore a una Web farm con più computer in modo semplice e senza modifiche complesse alla logica dell'applicazione.

## <a name="features-of-aspnet-web-forms"></a>Funzionalità di ASP.NET Web Forms

- **Controlli server**: i controlli server Web ASP.NET sono oggetti in pagine Web ASP.NET che vengono eseguite quando viene richiesta la pagina e che eseguono il rendering del markup nel browser. Molti controlli server Web sono simili a elementi HTML noti, quali pulsanti e caselle di testo. Altri controlli includono un comportamento complesso, ad esempio controlli calendario, e controlli che è possibile usare per connettersi alle origini dati e visualizzare i dati.
- **Pagine master**: le pagine master di ASP.NET consentono di creare un layout coerente per le pagine dell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard da applicare a tutte le pagine (o a un gruppo di pagine) dell'applicazione. È quindi possibile creare singole pagine di contenuto per il contenuto da visualizzare. Quando gli utenti richiedono le pagine di contenuto, si uniscono con la pagina master per produrre l'output che combina il layout della pagina master con il contenuto dalla pagina contenuto.
- **Uso di data**-ASP.NET offre molte opzioni per l'archiviazione, il recupero e la visualizzazione dei dati. In un'applicazione Web Form ASP.NET è possibile utilizzare i controlli associati a dati per automatizzare la presentazione o l'input dei dati negli elementi dell'interfaccia utente della pagina Web, ad esempio tabelle, caselle di testo ed elenchi a discesa.
- **Membership**-ASP.NET Identity archivia le credenziali degli utenti in un database creato dall'applicazione. Quando gli utenti accedono, l'applicazione convalida le proprie credenziali leggendo il database. La cartella *account* del progetto contiene i file che implementano le varie parti di appartenenza: registrazione, accesso, modifica di una password e autorizzazione dell'accesso. ASP.NET Web Forms supporta inoltre OAuth e OpenID. Questi miglioramenti dell'autenticazione consentono agli utenti di accedere al sito usando le credenziali esistenti, da account come Facebook, Twitter, Windows Live e Google. Per impostazione predefinita, il modello crea un database di appartenenza utilizzando un nome di database predefinito in un'istanza di SQL Server Express database locale, il server di database di sviluppo che include Visual Studio Express 2013 per il Web.
- **Framework client e script client**: è possibile migliorare le funzionalità basate su server di ASP.NET includendo la funzionalità script client nelle pagine Web Form di ASP.NET. È possibile utilizzare lo script client per fornire agli utenti un'interfaccia utente più completa e reattiva. È inoltre possibile utilizzare lo script client per eseguire chiamate asincrone al server Web durante l'esecuzione di una pagina nel browser.
- **Routing**-URL routing consente di configurare un'applicazione in modo che accetti URL di richiesta che non eseguono il mapping ai file fisici. Un URL di richiesta è semplicemente l'URL che un utente immette nel browser per trovare una pagina nel sito Web. Si usa il routing per definire URL semanticamente significativi per gli utenti e che possono essere utili per l'ottimizzazione del motore di ricerca (SEO).
- **Gestione dello stato**: ASP.NET Web Form include diverse opzioni che consentono di mantenere i dati sia in base a ogni pagina che a livello di applicazione.
- **Sicurezza**: una parte importante dello sviluppo di un'applicazione più sicura consiste nel comprendere le minacce. Microsoft ha sviluppato un modo per classificare le minacce: spoofing, manomissioni, ripudio, divulgazione di informazioni, Denial of Service, elevazione dei privilegi (STRIDE). In ASP.NET Web Forms è possibile aggiungere punti di estendibilità e opzioni di configurazione che consentono di personalizzare diversi comportamenti di sicurezza in Web Form ASP.NET.
- **Prestazioni**: le prestazioni possono essere un fattore chiave in un sito Web o in un progetto di successo. ASP.NET Web Forms consente di modificare le prestazioni relative all'elaborazione di controlli di pagine e server, alla gestione dello stato, all'accesso ai dati, alla configurazione e al caricamento delle applicazioni e a procedure di codifica efficienti.
- **Internazionalizzazione**: ASP.NET Web Forms consente di creare pagine Web in grado di ottenere contenuto e altri dati in base all'impostazione della lingua preferita per il browser o in base alla scelta della lingua esplicita dell'utente. Il contenuto e altri dati sono detti risorse e tali dati possono essere archiviati in file di risorse o in altre origini. In una pagina Web Form ASP.NET è possibile configurare i controlli per ottenere i valori delle proprietà dalle risorse. In fase di esecuzione, le espressioni di risorsa vengono sostituite dalle risorse dal file di risorse localizzato appropriato.
- **Debug e gestione degli errori**: ASP.NET include funzionalità che consentono di diagnosticare i problemi che potrebbero verificarsi nell'applicazione Web Form. Il debug e la gestione degli errori sono supportati all'interno di Web Form ASP.NET in modo che le applicazioni vengano compilate ed eseguite in modo efficiente.
- **Distribuzione e hosting**: Visual Studio, ASP.NET, Azure e IIS offrono strumenti che semplificano il processo di distribuzione e hosting dell'applicazione Web Form.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidere quando creare un'applicazione Web Form

È necessario considerare attentamente se implementare un'applicazione Web utilizzando il modello Web Form ASP.NET o un altro modello, ad esempio il framework ASP.NET MVC. Il framework MVC non sostituisce il modello Web Form. Per le applicazioni Web è infatti possibile utilizzare uno dei due. Prima di decidere di utilizzare il modello Web Form o il framework MVC per un sito Web specifico, valutare i vantaggi di ogni approccio.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su Web Form

Il framework basato su Web Form offre i seguenti vantaggi:

- Supporta un modello di eventi che mantiene lo stato su HTTP, con conseguenti vantaggi per lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi supportati in centinaia di controlli server.
- Utilizza un modello Page Controller che aggiunge funzionalità a singole pagine. Per ulteriori informazioni, vedere la pagina relativa al [controller di pagina](https://go.microsoft.com/fwlink/?LinkId=106359 "Controller di pagina") sul sito Web MSDN.
- USA i moduli basati sullo stato di visualizzazione o sul server, che possono semplificare la gestione delle informazioni sullo stato.
- È appropriato per i team di sviluppatori e designer Web di piccole dimensioni che desiderano usufruire della disponibilità di numerosi componenti per lo sviluppo rapido delle applicazioni.
- In generale, è meno complesso per lo sviluppo di applicazioni, perché i componenti (la classe della **pagina** , i controlli e così via) sono strettamente integrati e in genere richiedono meno codice rispetto al modello MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i seguenti vantaggi:

- Rende più facile gestire la complessità mediante la suddivisione di un'applicazione in un modello, una visualizzazione e un controller.
- Non utilizza lo stato di visualizzazione o form basati su server. Il framework MVC risulta in tal modo lo strumento ideale per gli sviluppatori che desiderano disporre del controllo completo sul comportamento di un'applicazione.
- Utilizza un modello Front Controller che elabora le richieste dell'applicazione Web attraverso un singolo controller, consentendo in tal modo di progettare un'applicazione dotata del supporto per un'infrastruttura di routing avanzata. Per ulteriori informazioni, vedere [front controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controller anteriore") nel sito Web MSDN.
- Offre un supporto più idoneo per lo sviluppo basato su test (TDD, Test-Driven Development).
- Funziona bene per le applicazioni Web supportate da grandi team di sviluppatori e progettisti Web che necessitano di un elevato livello di controllo sul comportamento dell'applicazione.
