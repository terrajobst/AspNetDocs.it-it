---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: elenco di prodotti | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 4 viene illustrata la lista dei prodotti con il contr GridView...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566983"
---
# <a name="part-4-listing-products"></a>Parte 4: elenco di prodotti

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella parte 4 viene illustrata la lista dei prodotti con il controllo GridView.

## <a id="_Toc260221670"></a>Elenco di prodotti con il controllo GridView

Iniziamo a implementare la pagina Products. aspx facendo clic con il pulsante destro del mouse sulla soluzione e scegliendo "Aggiungi" e "nuovo elemento".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Scegliere "Web Form utilizzando la pagina master" e immettere il nome della pagina Products. aspx ".

Fare clic su "Aggiungi".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Scegliere quindi la cartella "stili" in cui è stata posizionata la pagina Site. master e selezionarla dalla finestra "contenuto della cartella".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Fare clic su "OK" per creare la pagina.

Il database viene popolato con i dati del prodotto, come illustrato di seguito.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Dopo aver creato la pagina, si userà di nuovo un'origine dati dell'entità per accedere ai dati del prodotto, ma in questo caso è necessario selezionare le entità del prodotto ed è necessario limitare gli elementi restituiti solo a quelli per la categoria selezionata.

A tale scopo, si indica a EntityDataSource di generare automaticamente la clausola WHERE e si specificherà il WhereParameter.

Si noterà che, quando sono state create le voci di menu nel menu "Product Category", il collegamento è stato compilato dinamicamente aggiungendo CategoryID alla QueryString per ogni collegamento. Si indica all'origine dati dell'entità di derivare il parametro WHERE dal parametro QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Successivamente, verrà configurato il controllo ListView per visualizzare un elenco di prodotti. Per creare un'esperienza di acquisto ottimale, verranno comprimete diverse funzionalità concise in ogni singolo prodotto visualizzato nella ListVew.

- Il nome del prodotto sarà un collegamento alla visualizzazione dei dettagli del prodotto.
- Verrà visualizzato il prezzo del prodotto.
- Verrà visualizzata un'immagine del prodotto che verrà selezionata dinamicamente da una directory di immagini del catalogo nell'applicazione.
- Si includerà un collegamento per aggiungere immediatamente il prodotto specifico al carrello acquisti.

Ecco il markup per l'istanza del controllo ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Vengono compilati in modo dinamico diversi collegamenti per ogni prodotto visualizzato.

Inoltre, prima di testare la nuova pagina, è necessario creare la struttura di directory per le immagini del catalogo dei prodotti come indicato di seguito.

![](tailspin-spyworks-part-4/_static/image1.png)

Una volta accessibili le immagini del prodotto, è possibile testare la pagina dell'elenco di prodotti.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Dal home page del sito, fare clic su uno dei collegamenti elenco categorie.

![](tailspin-spyworks-part-4/_static/image6.jpg)

A questo punto è necessario implementare la pagina ProductDetails. aspx e la funzionalità AddToCart.

Usare file-&gt;nuovo per creare un nome di pagina ProductDetails. aspx usando la pagina master del sito come in precedenza.

Si utilizzerà di nuovo un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si utilizzerà un controllo ASP.NET FormView per visualizzare i dati del prodotto come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Non preoccuparti se la formattazione sembra un po' divertente. Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verranno implementate in un secondo momento.

Il carrello acquisti rappresenterà la logica più complessa nell'applicazione. Per iniziare, usare file-&gt;nuovo per creare una pagina denominata MyShoppingCart. aspx.

Si noti che non si sta scegliendo il nome ShoppingCart. aspx.

Il database contiene una tabella denominata "ShoppingCart". Quando è stato generato un Entity Data Model è stata creata una classe per ogni tabella nel database. Pertanto, il Entity Data Model ha generato una classe di entità denominata "ShoppingCart". È possibile modificare il modello in modo che sia possibile usare tale nome per l'implementazione del carrello acquisti o estenderlo in base alle esigenze, ma è necessario invece selezionare semplicemente un nome che eviterà il conflitto.

È anche importante notare che verrà creato un semplice carrello acquisti e verrà incorporata la logica del carrello della spesa con la visualizzazione del carrello. Microsoft potrebbe anche scegliere di implementare il carrello acquisti in un livello aziendale completamente separato.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-3.md)
> [Successivo](tailspin-spyworks-part-5.md)
