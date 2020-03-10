---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Panoramica di ASP.NET MVC | Microsoft Docs
author: microsoft
description: Informazioni sulle differenze tra l'applicazione ASP.NET MVC e le applicazioni Web Form ASP.NET. Informazioni su come decidere quando compilare un'applicazione MVC ASP.NET.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 73965c71f37de13e3813df089a253fde528ea7ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541559"
---
# <a name="aspnet-mvc-overview"></a>Panoramica di ASP.NET MVC

[Microsoft](https://github.com/microsoft)

> Informazioni sulle differenze tra l'applicazione ASP.NET MVC e le applicazioni Web Form ASP.NET. Informazioni su come decidere quando compilare un'applicazione MVC ASP.NET.

Il modello architetturale MVC (Model-View-Controller) suddivide un'applicazione in tre componenti principali: il modello, la visualizzazione e il controller. Il framework ASP.NET MVC fornisce un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su MVC. ASP.NET MVC è un framework di presentazione leggero e facile da testare che, come le applicazioni basate su Web Form, è integrato con funzionalità ASP.NET esistenti, quali le pagine master e l'autenticazione basata sull'appartenenza. Il framework MVC è definito nello spazio dei nomi **System. Web. Mvc** ed è una parte fondamentale supportata dello spazio dei nomi **System. Web** .   
  
MVC è un modello di progettazione standard con cui molti sviluppatori hanno familiarità. Alcuni tipi di applicazioni Web trarranno il massimo vantaggio dal framework MVC, mentre altri continueranno a utilizzare il modello ASP.NET tradizionale basato su Web Form e postback e altri ancora adotteranno una combinazione di entrambi gli approcci, dato che nessuno dei due esclude l'altro.   
  
Il framework MVC include i seguenti componenti:

[![richiamare un'azione del controller che prevede un valore di parametro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: richiamo di un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-overview/_static/image2.png))

- **Modelli**. Gli oggetti modello sono le parti dell'applicazione che implementano la logica per il dominio di dati dell'applicazione. Spesso gli oggetti modello recuperano e archiviano lo stato del modello in un database. Ad esempio, un oggetto prodotto potrebbe recuperare informazioni da un database, operare su di esso e quindi scrivere le informazioni aggiornate in una tabella Products in SQL Server.

Nelle applicazioni di dimensioni ridotte il modello costituisce spesso una separazione concettuale anziché fisica. Se, ad esempio, l'applicazione legge un set di dati e lo invia alla visualizzazione, l'applicazione non dispone di un livello di modello fisico e di classi associate. In tal caso, il set di dati assume il ruolo di un oggetto modello.

- **Viste**. Le visualizzazioni sono i componenti che visualizzano l'interfaccia utente dell'applicazione. Questa interfaccia utente viene in genere creata in base ai dati del modello. Un esempio è una visualizzazione di modifica di una tabella Products che visualizza caselle di testo, elenchi a discesa e caselle di controllo in base allo stato corrente di un oggetto Products.

- **Controller**. I controller sono i componenti che gestiscono l'interazione dell'utente, utilizzano il modello e selezionano infine una visualizzazione per il rendering dell'interfaccia utente. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente. Il controller, ad esempio, gestisce i valori della stringa di query e passa questi valori al modello, che a sua volta esegue una query sul database usando i valori.

Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo nel contempo un accoppiamento di tipo loose tra questi elementi. Il modello specifica la posizione in cui ogni tipo di logica deve trovarsi nell'applicazione. La logica dell'interfaccia utente risiede nella vista. La logica di input risiede nel controller. La logica di business risiede nel modello. Questa separazione consente di gestire la complessità al momento della compilazione di un'applicazione, in quanto permette di concentrarsi su un aspetto dell'implementazione alla volta. È ad esempio possibile concentrarsi sulla visualizzazione senza dipendere dalla logica di business.   
  
Oltre a semplificare la gestione della complessità, il modello MVC rende più facile eseguire il test di applicazioni rispetto a un'applicazione Web ASP.NET basata su Web Form. Ad esempio, in un'applicazione Web ASP.NET basata su Web Form viene utilizzata un'unica classe per visualizzare l'output e per rispondere all'input dell'utente. La scrittura di test automatizzati per applicazioni ASP.NET basate su Web Form può risultare complessa in quanto per eseguire il test di una singola pagina è necessario creare istanze della classe di pagina, di tutti i relativi controlli figlio e delle classi dipendenti aggiuntive nell'applicazione. Poiché per eseguire la pagina vengono create istanze per un numero elevato di classi, può essere difficile scrivere test indirizzati esclusivamente su singole parti dell'applicazione. I test per le applicazioni ASP.NET basate su Web Form possono essere pertanto più difficili da implementare rispetto ai test in un'applicazione MVC. Per i test in un'applicazione ASP.NET basata su Web Form è inoltre necessario disporre di un server Web. Il framework MVC disaccoppia i componenti e utilizza in modo massiccio le interfacce, rendendo pertanto possibile il test di singoli componenti in maniera isolata dal resto del framework.   
  
L'accoppiamento di tipo loose tra i tre componenti principali di un'applicazione MVC favorisce inoltre lo sviluppo parallelo. Ad esempio, uno sviluppatore può lavorare sulla vista, un secondo sviluppatore può lavorare sulla logica del controller e un terzo sviluppatore può concentrarsi sulla logica di business nel modello.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidere quando creare un'applicazione MVC

È necessario valutare con attenzione se implementare un'applicazione Web tramite il framework ASP.NET MVC o il modello Web Form ASP.NET. Il framework MVC non sostituisce il modello Web Form. Per le applicazioni Web è infatti possibile utilizzare uno dei due. Se si dispone di applicazioni basate su Web Form esistenti, queste continueranno a funzionare normalmente.   
  
Prima di decidere di utilizzare il framework MVC o il modello Web Form per un sito Web specifico, considerare i vantaggi offerti da ciascun approccio.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i seguenti vantaggi:

- Rende più facile gestire la complessità mediante la suddivisione di un'applicazione in un modello, una visualizzazione e un controller.
- Non utilizza lo stato di visualizzazione o form basati su server. Il framework MVC risulta in tal modo lo strumento ideale per gli sviluppatori che desiderano disporre del controllo completo sul comportamento di un'applicazione.
- Utilizza un modello Front Controller che elabora le richieste dell'applicazione Web attraverso un singolo controller, consentendo in tal modo di progettare un'applicazione dotata del supporto per un'infrastruttura di routing avanzata. Per ulteriori informazioni, vedere [front controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controller anteriore") nel sito Web MSDN.
- Offre un supporto più idoneo per lo sviluppo basato su test (TDD, Test-Driven Development).
- Funziona bene per le applicazioni Web supportate da grandi team di sviluppatori e progettisti Web che necessitano di un elevato livello di controllo sul comportamento dell'applicazione.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su Web Form

Il framework basato su Web Form offre i seguenti vantaggi:

- Supporta un modello di eventi che mantiene lo stato su HTTP, con conseguenti vantaggi per lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi supportati in centinaia di controlli server.
- Utilizza un modello Page Controller che aggiunge funzionalità a singole pagine. Per ulteriori informazioni, vedere la pagina relativa al [controller di pagina](https://go.microsoft.com/fwlink/?LinkId=106359 "Controller di pagina") sul sito Web MSDN.
- USA i moduli basati sullo stato di visualizzazione o sul server, che possono semplificare la gestione delle informazioni sullo stato.
- È appropriato per i team di sviluppatori e designer Web di piccole dimensioni che desiderano usufruire della disponibilità di numerosi componenti per lo sviluppo rapido delle applicazioni.
- In generale, è meno complesso per lo sviluppo di applicazioni, perché i componenti (la classe della **pagina** , i controlli e così via) sono strettamente integrati e in genere richiedono meno codice rispetto al modello MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funzionalità del framework ASP.NET MVC

Il framework ASP.NET MVC offre le seguenti funzionalità:

- Separazione delle attività dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), testabilità e sviluppo basato su test (TDD) per impostazione predefinita. Tutti i contratti principali nel framework MVC sono basati sull'interfaccia e possono essere sottoposti a test tramite oggetti fittizi, ovvero oggetti simulati che imitano il comportamento degli oggetti effettivi nell'applicazione. È possibile eseguire uno unit test dell'applicazione senza dovere eseguire i controller in un processo ASP.NET. Le attività di unit test risultano in tal modo veloci e flessibili. È possibile utilizzare qualsiasi framework per unit test compatibile con .NET Framework.
- Framework estendibile e collegabile. I componenti del framework ASP.NET MVC sono progettati per essere sostituiti o personalizzati con facilità. È possibile collegare il motore di visualizzazione, i criteri di routing degli URL e la serializzazione dei parametri dei metodi di azione personalizzati, nonché altri componenti. Il framework ASP.NET MVC supporta inoltre l'utilizzo dei modelli contenitore Dependency Injection (DI) e Inversion of Control (IOC). Consente di inserire oggetti in una classe, invece di basarsi sulla classe per creare l'oggetto stesso. Il modello IOC specifica che, se un oggetto richiede un altro oggetto, il primo deve ottenere il secondo da un'origine esterna, ad esempio un file di configurazione. In questo modo, l'esecuzione di test risulta semplificata.
- Un potente componente di mapping URL che consente di compilare applicazioni con URL comprensibili e ricercabili. Gli URL non devono includere le estensioni di file e sono progettati per supportare i modelli di denominazione degli URL che risultano appropriati per l'ottimizzazione del motore di ricerca (SEO) e l'indirizzamento REST (Representational State Transfer).
- Supporto per l'utilizzo del markup in file di pagine ASP.NET (aspx), file di controlli utente (ascx) e file di markup di pagine master (master) come modelli di visualizzazione. È possibile utilizzare le funzionalità ASP.NET esistenti con il framework ASP.NET MVC, ad esempio le pagine master annidate, le espressioni inline (&lt;% =%&gt;), i controlli server dichiarativi, i modelli, l'associazione dati, la localizzazione e così via.
- Supporto per le funzionalità ASP.NET esistenti. ASP.NET MVC consente di utilizzare numerose funzionalità, tra cui l'autenticazione basata su form e l'autenticazione di Windows, l'autorizzazione di URL, l'appartenenza e i ruoli, la memorizzazione nella cache di output e dati, la gestione dello stato di sessione e profilo, il monitoraggio dell'integrità, il sistema di configurazione e l'architettura del provider.
