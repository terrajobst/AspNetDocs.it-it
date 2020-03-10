---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: file-> nuovo progetto | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 1 vengono illustrati i Cenni preliminari e file/nuovo progetto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636129"
---
# <a name="part-1-file--new-project"></a>Parte 1: file-> nuovo progetto

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 1 vengono illustrati i Cenni preliminari e file/nuovo progetto.

## <a id="_Toc260221666"></a>Panoramica

Questa esercitazione è un'introduzione ai Web Form ASP.NET. Inizieremo lentamente, quindi l'esperienza di sviluppo Web a livello di principianti è accettabile.

L'applicazione che verrà compilata è un semplice negozio online.

![](tailspin-spyworks-part-1/_static/image1.jpg)

I visitatori possono sfogliare i prodotti in base alla categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Possono visualizzare un singolo prodotto e aggiungerlo al carrello:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Possono esaminare il carrello, rimuovendo tutti gli elementi che non vogliono più:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuando con l'estrazione verrà richiesto di

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Dopo l'ordinamento, viene visualizzata una schermata di conferma semplice:

![](tailspin-spyworks-part-1/_static/image7.jpg)

Si inizierà con la creazione di un nuovo progetto WebForms di ASP.NET in Visual Studio 2010 e si aggiungeranno in modo incrementale le funzionalità per creare un'applicazione completa funzionante. Verranno illustrati l'accesso al database, le visualizzazioni elenco e griglia, le pagine di aggiornamento dei dati, la convalida dei dati, l'utilizzo di pagine master per un layout di pagina coerente, AJAX, convalida, appartenenza utente e altro ancora.

È possibile seguire la procedura dettagliata oppure è possibile scaricare l'applicazione completata da [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

È possibile usare Visual Studio 2010 o la versione gratuita di Visual Web Developer 2010 da [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Per compilare l'applicazione, è possibile utilizzare SQL Server o la SQL Server Express gratuita per ospitare il database.

## <a id="_Toc260221667"></a>File/nuovo progetto

Per iniziare, selezionare il nuovo progetto dal menu file in Visual Studio. Verrà visualizzata la finestra di dialogo nuovo progetto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Selezionare il gruppo di modelli C# visivi e Web a sinistra, quindi scegliere il modello "applicazione Web ASP.NET" nella colonna centrale. Denominare il progetto TailspinSpyworks e premere il pulsante OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Verrà creato il progetto. Verranno ora esaminate le cartelle incluse nell'applicazione nell'Esplora soluzioni sul lato destro.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La soluzione vuota non è completamente vuota, ma aggiunge una struttura di cartelle di base:

![](tailspin-spyworks-part-1/_static/image1.png)

Prendere nota delle convenzioni implementate dal modello di progetto ASP.NET 4 predefinito.

- La cartella "account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza di NET.
- La cartella "Scripts" funge da repository per i file JavaScript lato client e i file jQuery. js di base vengono resi disponibili per impostazione predefinita.
- La cartella "stili" viene usata per organizzare gli oggetti visivi del sito Web (fogli di stile CSS)

Quando si preme F5 per eseguire l'applicazione e si esegue il rendering della pagina default. aspx, viene visualizzato quanto segue.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Il primo miglioramento dell'applicazione consiste nel sostituire il file Style. CSS dal modello Web Forms predefinito con le classi CSS e i file di immagine associati che eseguiranno il rendering dell'oggetto visivo asthetics che si vuole per l'applicazione SpyWorks di Tilt.

Dopo questa operazione, il rendering della pagina default. aspx è simile al seguente.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Si notino i collegamenti all'immagine nella parte superiore destra della pagina e le voci di menu aggiunte alla pagina master. Solo i collegamenti "Accedi" e "account" indicano le pagine esistenti (generate dal modello predefinito) e il resto delle pagine che si implementeranno durante la compilazione dell'applicazione.

La pagina master verrà rilocata anche nella directory Styles. Anche se questa è solo una preferenza, può rendere le cose più semplici se si decide di rendere l'applicazione "skinable" in futuro.

Al termine di questa operazione, sarà necessario modificare i riferimenti della pagina master in tutti i file con estensione aspx generati dalle pagine Web Forms ASP.NET predefinite.

> [!div class="step-by-step"]
> [avanti](tailspin-spyworks-part-2.md)
