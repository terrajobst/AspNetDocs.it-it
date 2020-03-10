---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: panoramica e nuovo progetto di file > | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 1 vengono illustrati i Cenni preliminari e > nuovo progetto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559983"
---
# <a name="part-1-overview-and-file-new-project"></a>Parte 1: panoramica e nuovo progetto di > file

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella parte 1 vengono illustrati i Cenni preliminari e&gt;nuovo progetto.

## <a name="overview"></a>Panoramica

MVC Music Store è un'applicazione di esercitazione che introduce e illustra in modo dettagliato come utilizzare ASP.NET MVC e Visual Web Developer per lo sviluppo Web. Inizieremo lentamente, quindi l'esperienza di sviluppo Web a livello di principianti è accettabile.

L'applicazione che verrà compilata è un semplice negozio di musica. L'applicazione include tre parti principali: acquisti, estrazione e amministrazione.

![](mvc-music-store-part-1/_static/image1.jpg)

I visitatori possono sfogliare gli album per genere:

![](mvc-music-store-part-1/_static/image2.jpg)

Possono visualizzare un singolo album e aggiungerlo al carrello:

![](mvc-music-store-part-1/_static/image3.jpg)

Possono esaminare il carrello, rimuovendo tutti gli elementi che non vogliono più:

![](mvc-music-store-part-1/_static/image4.jpg)

Continuando con l'estrazione verrà richiesto di effettuare l'accesso o di registrarsi per un account utente.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Dopo aver creato un account, è possibile completare l'ordine compilando le informazioni di spedizione e pagamento. Per semplificare le cose, stiamo eseguendo una promozione straordinaria: tutti gli elementi sono gratuiti se entrano nel codice promozione "FREE".

![](mvc-music-store-part-1/_static/image5.jpg)

Dopo l'ordinamento, viene visualizzata una schermata di conferma semplice:

![](mvc-music-store-part-1/_static/image6.jpg)

Oltre alle pagine rivolte ai clienti, viene anche compilata una sezione amministratore che mostra un elenco di album da cui gli amministratori possono creare, modificare ed eliminare gli album:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. file-&gt; nuovo progetto

### <a name="installing-the-software"></a>Installazione del software

Questa esercitazione inizierà con la creazione di un nuovo progetto ASP.NET MVC 3 con la versione gratuita di Visual Web Developer 2010 Express (gratuita), quindi verranno aggiunte in modo incrementale le funzionalità per creare un'applicazione completa funzionante. Verranno illustrati l'accesso al database, gli scenari di inserimento dei moduli, la convalida dei dati, l'utilizzo di pagine master per un layout di pagina coerente, l'utilizzo di AJAX per gli aggiornamenti e la convalida della pagina, l'accesso degli utenti e altro ancora.

È possibile seguire la procedura dettagliata oppure è possibile scaricare l'applicazione completata da [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Per compilare l'applicazione, è possibile utilizzare Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versione gratuita di Visual Studio 2010). Verrà usato il SQL Server Compact (anche gratuito) per ospitare il database. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.

- [Prerequisiti di Visual Studio Web Developer Express SP1]
- [Aggiornamento degli strumenti di ASP.NET MVC 3]
- [SQL Server Compact 4,0]-inclusione del supporto per Runtime e strumenti

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Creazione di un nuovo progetto MVC 3 ASP.NET

Per iniziare, selezionare "nuovo progetto" dal menu file in Visual Web Developer. Verrà visualizzata la finestra di dialogo nuovo progetto.

![](mvc-music-store-part-1/_static/image5.png)

Selezionare il gruppo Visual C# -&gt; Web Templates a sinistra, quindi scegliere il modello "ASP.NET MVC 3 Web Application" nella colonna centrale. Denominare il progetto MvcMusicStore e premere il pulsante OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Verrà visualizzata una finestra di dialogo secondaria che consente di apportare alcune impostazioni specifiche di MVC per il progetto. Selezionare le opzioni seguenti:

Modello di progetto-selezionare vuoto

Motore di visualizzazione-seleziona Razor

Usa markup semantico HTML5-selezionato

Verificare che le impostazioni siano indicate di seguito, quindi fare clic sul pulsante OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Verrà creato il progetto. Verranno ora esaminate le cartelle che sono state aggiunte all'applicazione nella Esplora soluzioni sul lato destro.

![](mvc-music-store-part-1/_static/image10.jpg)

Il modello MVC 3 vuoto non è completamente vuoto, ma aggiunge una struttura di cartelle di base:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC usa alcune convenzioni di denominazione di base per i nomi delle cartelle:

| **Cartella** | **Scopo** |
| --- | --- |
| **/Controllers** | I controller rispondono all'input dal browser, decidono cosa fare con esso e restituiscono la risposta all'utente. |
| **/Views** | Viste che contengono i modelli dell'interfaccia utente |
| **/Models** | I modelli contengono e modificano i dati |
| **/Content** | Questa cartella include immagini, CSS e qualsiasi altro contenuto statico |
| **/Scripts** | Questa cartella include i file JavaScript |

Queste cartelle sono incluse anche in un'applicazione MVC ASP.NET vuota perché per impostazione predefinita il framework ASP.NET MVC usa un approccio di "Convenzione sulla configurazione" ed esegue alcuni presupposti predefiniti in base alle convenzioni di denominazione delle cartelle. Per impostazione predefinita, ad esempio, i controller cercano le visualizzazioni nella cartella Views senza che sia necessario specificarlo in modo esplicito nel codice. Con le convenzioni predefinite è possibile ridurre la quantità di codice che è necessario scrivere e consentire agli altri sviluppatori di comprendere il progetto in modo più semplice. Queste convenzioni verranno spiegate più in base alla compilazione dell'applicazione.

> [!div class="step-by-step"]
> [avanti](mvc-music-store-part-2.md)
