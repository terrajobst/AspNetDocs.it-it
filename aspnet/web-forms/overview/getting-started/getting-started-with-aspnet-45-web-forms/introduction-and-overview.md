---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introduzione con Web Form ASP.NET 4,7 e Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni dettagliate illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615459"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Introduzione con Web Form ASP.NET 4,5 e Visual Studio 2017

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Questa serie di esercitazioni illustra come compilare un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introduzione

Questa serie di esercitazioni illustra la creazione di un'applicazione Web Form ASP.NET con Visual Studio 2017 e ASP.NET 4,5. Si creerà un'applicazione denominata **Wingtip Toys** , un sito Web di una vetrina semplificata che vende gli elementi online. Durante la serie vengono evidenziate le nuove funzionalità di ASP.NET 4,5.

### <a name="target-audience"></a>Destinatari

Gli sviluppatori che non hanno familiarità con ASP.NET Web Form sono i destinatari di questa serie di esercitazioni.

È necessario conoscere le aree seguenti:

- Programmazione orientata a oggetti (OOP) e linguaggi
- Sviluppo Web (HTML, CSS, JavaScript)
- Database relazionali
- Architettura a più livelli

Per esaminare queste aree, provare a studiare il contenuto seguente:

- [Introduzione con VisualC#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Sviluppo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database)
- [Architettura a più livelli](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funzionalità dell'applicazione

Le funzionalità del Web Form ASP.NET presentate in questa serie includono:

- Progetto di applicazione Web (non progetto sito Web)
- Web Form
- Pagine master, configurazione
- Bootstrap
- Entity Framework Code First, database locale
- Convalida della richiesta
- Controlli dati fortemente tipizzati
- Associazione di modelli
- Annotazioni dei dati
- Provider di valori
- SSL e OAuth
- ASP.NET Identity, configurazione e autorizzazione
- Convalida non intrusiva
- Routing
- Gestione degli errori di ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scenari e attività delle applicazioni

Le attività della serie di esercitazioni includono:

- Creazione, revisione ed esecuzione di un nuovo progetto
- Creazione di una struttura di database
- Inizializzazione e seeding di un database
- Personalizzazione dell'interfaccia utente con stili, grafica e una pagina master
- Aggiunta di pagine e spostamento
- Visualizzazione dei dettagli del menu e dei dati del prodotto
- Creazione di un carrello acquisti
- Aggiunta del supporto per SSL e OAuth
- Aggiunta di un metodo di pagamento
- Inclusione di un ruolo amministratore e di un utente per l'applicazione
- Limitazione dell'accesso a pagine e cartelle specifiche
- Caricamento di un file nell'applicazione Web
- Implementazione della convalida dell'input
- Registrazione delle route per l'applicazione Web
- Implementazione della gestione degli errori e della registrazione degli errori

## <a name="overview"></a>Panoramica di

Questa serie di esercitazioni è destinata a chi ha familiarità con i concetti di programmazione, ma è una novità di ASP.NET Web Form. Se si ha già familiarità con ASP.NET Web Forms, questa serie può comunque essere utile per apprendere le nuove funzionalità di ASP.NET 4,5. Per i lettori che non hanno familiarità con i concetti di programmazione e i Web Form ASP.NET, vedere le esercitazioni aggiuntive su Web Form disponibili nella sezione [Introduzione](../../../index.md) del sito Web ASP.NET.

Il ASP.NET 4,5 fornito in questa serie di esercitazioni include le seguenti funzionalità:

- Interfaccia utente semplice per la creazione di progetti che offre [supporto per molti framework ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), layout, tema e Framework di progettazione reattivo.
- [ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software di hosting Web diverso da IIS.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Un aggiornamento alla Entity Framework consente di:
  - Recuperare e modificare i dati come oggetti fortemente tipizzati
  - Accesso ai dati in modo asincrono
  - Gestione degli errori di connessione temporanei
  - Log istruzioni SQL

Per un elenco completo delle funzionalità di ASP.NET 4,5, vedere [ASP.NET and Web Tools per Visual Studio 2013 Note sulla versione](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Applicazione di esempio Wingtip Toys

Gli screenshot seguenti rientrano nell'applicazione Web Form ASP.NET creata in questa serie di esercitazioni. Quando si esegue l'applicazione in Visual Studio, viene visualizzata la Home page Web seguente.

![Wingtip Toys-pagina predefinita](introduction-and-overview/_static/image1.png)

È possibile registrarsi come nuovo utente o accedere come utente esistente. La navigazione superiore include collegamenti alle categorie di prodotti e ai relativi prodotti dal database.

Se si seleziona **prodotti**, verranno visualizzati tutti i prodotti disponibili. 

![Giocattoli Wingtip-prodotti](introduction-and-overview/_static/image2.png)

Se si seleziona un prodotto specifico, vengono visualizzati i dettagli del prodotto.

![Wingtip Toys-dettagli sul prodotto](introduction-and-overview/_static/image3.png)

Un utente può registrarsi e accedere con la funzionalità predefinita del modello Web Form. Questa esercitazione illustra anche come eseguire l'accesso con un account Gmail esistente. Inoltre, è possibile accedere come amministratore per aggiungere e rimuovere prodotti dal database.

![Wingtip Toys-accedi](introduction-and-overview/_static/image4.png)

Dopo aver eseguito l'accesso come utente, è possibile aggiungere prodotti al carrello acquisti ed effettuare il checkout con PayPal. L'applicazione di esempio è progettata per funzionare in sandbox per sviluppatori di PayPal. Non si verifica alcuna transazione di denaro effettiva.

![Wingtip Toys-carrello acquisti](introduction-and-overview/_static/image5.png)

PayPal conferma le informazioni relative all'account, all'ordine e al pagamento.

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

Dopo la restituzione da PayPal, è possibile esaminare e completare l'ordine.

![Wingtip Toys-revisione degli ordini](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prerequisites

Prima di iniziare, verificare che nel computer sia installato il software seguente:

- [Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

Il .NET Framework viene installato automaticamente.

Questa serie di esercitazioni USA Microsoft Visual Studio Community 2017. Per completare questa serie di esercitazioni, è possibile usare tale o Microsoft Visual Studio 2017.

Si noti quanto segue su Visual Studio:

* In questa serie di esercitazioni, Microsoft Visual Studio 2017 e Microsoft Visual Studio Community 2017 sono denominati *Visual Studio* .

* Visual Studio 2017 è installato accanto a qualsiasi versione precedente già installata. I siti creati nelle versioni precedenti possono essere aperti in Visual Studio 2017 e continuano a essere aperti nelle versioni precedenti.

* La prima volta che si avvia Visual Studio si presuppone che siano state selezionate le impostazioni per *lo sviluppo Web* . Per altre informazioni, vedere [procedura: selezionare le impostazioni dell'ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il progetto Web presentato in questa serie di esercitazioni.

## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

 È possibile scaricare l'applicazione di esempio completata in qualsiasi momento dal sito degli esempi MSDN:

[Introduzione con Web form ASP.NET 4,5 Visual Studio 2013 e Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Questo download include gli elementi seguenti:

- Applicazione di esempio nella cartella *WingtipToys* .
- Risorse usate per creare l'applicazione di esempio nella cartella *WingtipToys-assets* nella cartella *WingtipToys* .

Il download è un file con *estensione zip* . Per visualizzare il progetto completato creato da questa serie di esercitazioni, trovare e *C#* selezionare la cartella nel file con estensione zip. Salvare la C# cartella nella cartella utilizzata per lavorare con i progetti di Visual Studio. Per impostazione predefinita, la cartella progetti di Visual Studio 2017 è:

<strong>C:\Users&#92;</strong>  <strong><em>&lt;username&gt;</em></strong> <strong>\source\repos</strong>

Rinominare la ***C#*** cartella in ***WingtipToys***.

> [!NOTE]
> Se è già presente una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente la cartella esistente prima di rinominare la *C#* cartella in *WingtipToys*.

Per eseguire il progetto completato, aprire la cartella *WingtipToys* e fare doppio clic sul file *WingtipToys. sln* . Visual Studio 2017 apre il progetto. Fare quindi clic con il pulsante destro del mouse sul file *default. aspx* in **Esplora soluzioni** e selezionare **Visualizza nel browser**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Eseguire un quiz Web Form ASP.NET per esaminare il contenuto

Dopo aver completato la serie di esercitazioni, eseguire un quiz per testare le proprie conoscenze e rafforzare i concetti chiave. Ogni domanda fornisce una spiegazione e collegamenti a istruzioni aggiuntive.

* [Quiz Web Form ASP.NET](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Supporto e commenti per l'esercitazione

Per domande e commenti, usare la sezione Q e una inclusa nella pagina [di esempio introduzione con ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).

I commenti su questa serie di esercitazioni sono benvenuti. Quando questa serie di esercitazioni viene aggiornata, viene eseguito ogni sforzo per prendere in considerazione le correzioni o i suggerimenti per i miglioramenti.

Se si verifica un errore, i messaggi di errore corrispondenti potrebbero generare confusione, senza alcuna spiegazione adeguata su come risolverlo. Per informazioni, è possibile consultare i [Forum di ASP.NET](https://forums.asp.net/). Un'altra fonte interessante è la sezione Q e una della pagina di esempio [Introduzione con ASP.NET 4,5 Web Forms e Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#). 

> [!div class="step-by-step"]
> [avanti](create-the-project.md)
