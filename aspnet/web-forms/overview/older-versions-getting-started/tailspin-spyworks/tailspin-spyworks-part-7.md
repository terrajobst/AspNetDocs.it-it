---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: aggiunta di funzionalità | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 7 aggiunge funzionalità aggiuntive, ad esempio Revie account...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641995"
---
# <a name="part-7-adding-features"></a>Parte 7: aggiunta di funzionalità

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 7 aggiunge funzionalità aggiuntive, ad esempio la revisione degli account, i revisioni del prodotto e i controlli utente "più diffusi" e "acquistati".

## <a id="_Toc260221673"></a>Aggiunta di funzionalità

Sebbene gli utenti possano esplorare il catalogo, inserire gli elementi nel carrello della spesa e completare il processo di checkout, sono disponibili diverse funzionalità di supporto che verranno incluse per migliorare il sito.

1. Revisione dell'account (elencare gli ordini inseriti e visualizzare i dettagli)
2. Aggiungere un contenuto specifico del contesto alla pagina iniziale.
3. Aggiungere una funzionalità per consentire agli utenti di esaminare i prodotti nel catalogo.
4. Creare un controllo utente per visualizzare gli elementi più diffusi e inserire il controllo nella pagina anteriore.
5. Creare un controllo utente "acquistato anche" e aggiungerlo alla pagina dei dettagli del prodotto.
6. Aggiungere una pagina di contatto.
7. Aggiungere una pagina informazioni su.
8. Errore globale

## <a id="_Toc260221674"></a>Revisione account

Nella cartella "account" creare due pagine. aspx una denominata Order. aspx e l'altra denominata OrderDetails. aspx

Orders. aspx utilizzerà i controlli GridView e EntityDataSource in modo molto simile a quanto già in precedenza.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSource seleziona i record della tabella Orders filtrata in base al nome utente (vedere il WhereParameter) impostato in una variabile di sessione quando il registro utente è in.

Si noti anche questi parametri in HyperlinkField di GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Questi specificano il collegamento alla visualizzazione dei dettagli degli ordini per ogni prodotto che specifica il campo OrderID come parametro QueryString nella pagina OrderDetails. aspx.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Si utilizzerà un controllo EntityDataSource per accedere agli ordini e a un controllo FormView per visualizzare i dati degli ordini e un altro oggetto EntityDataSource con GridView per visualizzare tutte le voci dell'ordine.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Nel file code-behind (OrderDetails.aspx.cs) sono disponibili due piccoli bit di manutenzione.

Prima di tutto è necessario assicurarsi che OrderDetails ottenga sempre un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

È anche necessario calcolare e visualizzare il totale degli ordini dalle voci.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Pagina iniziale

Si aggiungerà un contenuto statico alla pagina default. aspx.

Innanzitutto, creo una cartella "Content" (contenuto) e all'interno di essa una cartella images (e includo un'immagine da usare nel home page).

Nel segnaposto inferiore della pagina default. aspx aggiungere il markup seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Revisioni del prodotto

Prima di tutto verrà aggiunto un pulsante con un collegamento a un modulo che è possibile usare per immettere una revisione del prodotto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Si noti che il ProductID viene passato nella stringa di query

Quindi, aggiungiamo la pagina denominata ReviewAdd. aspx

In questa pagina verrà usato ASP.NET AJAX Control Toolkit. Se non è già stato fatto, è possibile scaricarlo da [DevExpress](http://devexpress.com/act) e sono disponibili informazioni aggiuntive sulla configurazione del Toolkit per l'uso con Visual Studio qui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

In modalità progettazione trascinare i controlli e i validator dalla casella degli strumenti e compilare un form come quello riportato di seguito.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Il markup sarà simile al seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ora che è possibile immettere le revisioni, visualizzare le revisioni nella pagina del prodotto.

Aggiungere questo markup alla pagina ProductDetails. aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Eseguendo ora l'applicazione e passando a un prodotto vengono visualizzate le informazioni sul prodotto, incluse le verifiche dei clienti.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Controllo elementi più diffusi (creazione di controlli utente)

Per aumentare le vendite nel sito Web, si aggiungeranno alcune funzionalità ai prodotti più diffusi o correlati.

Il primo di queste funzionalità sarà un elenco dei prodotti più diffusi nel catalogo prodotti.

Viene creato un "controllo utente" per visualizzare gli articoli più venduti nella home page dell'applicazione. Poiché si tratta di un controllo, è possibile usarlo in qualsiasi pagina semplicemente trascinando il controllo nella finestra di progettazione di Visual Studio su qualsiasi pagina desiderata.

In Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sul nome della soluzione e creare una nuova directory denominata "controlli". Sebbene non sia necessario, il progetto verrà organizzato creando tutti i controlli utente nella directory "Controls".

Fare clic con il pulsante destro del mouse sulla cartella controlli e scegliere "nuovo elemento":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Specificare un nome per il controllo "PopularItems". Si noti che l'estensione di file per i controlli utente è. ascx not. aspx.

Il controllo utente degli elementi più diffusi verrà definito come segue.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Qui viene usato un metodo non ancora usato in questa applicazione. Si sta usando il controllo Repeater e invece di usare un controllo origine dati il controllo Repeater viene associato ai risultati di una query LINQ to Entities.

Nel code-behind del nostro controllo lo facciamo come segue.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Si noti anche questa linea importante nella parte superiore del markup del controllo.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Poiché gli elementi più diffusi non verranno modificati in base ai minuti, è possibile aggiungere una direttiva che consente di migliorare le prestazioni dell'applicazione. Questa direttiva farà sì che il codice dei controlli venga eseguito solo quando l'output memorizzato nella cache del controllo scade. In caso contrario, verrà utilizzata la versione memorizzata nella cache dell'output del controllo.

A questo punto, è necessario includere il nuovo controllo nella pagina default. aspx.

Usare il trascinamento della selezione per inserire un'istanza del controllo nella colonna aperta del modulo predefinito.

![](tailspin-spyworks-part-7/_static/image5.jpg)

A questo punto, quando si esegue l'applicazione, il home page Visualizza gli elementi più diffusi.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>Controllo "also purchased" (controlli utente con parametri)

Il secondo controllo utente che verrà creato apporterà una vendita indicativa al livello successivo aggiungendo la specificità del contesto.

La logica per calcolare i primi elementi "acquistati anche" è non semplice.

Il controllo "anche acquistato" Seleziona i record OrderDetails (precedentemente acquistati) per il ProductID attualmente selezionato e acquisisce gli OrderID per ogni ordine univoco rilevato.

Verranno quindi selezionati i prodotti di tutti gli ordini e verranno sommate le quantità acquistate. Verranno ordinati i prodotti in base a tale quantità somma e verranno visualizzati i primi cinque elementi.

Data la complessità di questa logica, questo algoritmo verrà implementato come stored procedure.

Il T-SQL per il stored procedure è il seguente.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Si noti che questo stored procedure (SelectPurchasedWithProducts) era presente nel database quando è stato incluso nell'applicazione e quando è stato generato il Entity Data Model è stato specificato che, oltre alle tabelle e alle viste necessarie, il Entity Data Model deve includere questa stored procedure.

Per accedere al stored procedure dalla Entity Data Model è necessario importare la funzione.

Fare doppio clic sul Entity Data Model in Esplora soluzioni per aprirlo nella finestra di progettazione e aprire il browser modello, quindi fare clic con il pulsante destro del mouse nella finestra di progettazione e scegliere "Aggiungi importazione funzioni".

![](tailspin-spyworks-part-7/_static/image1.png)

In questo modo verrà aperta questa finestra di dialogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Compilare i campi come indicato sopra, selezionando "SelectPurchasedWithProducts" e usare il nome della procedura per il nome della funzione importata.

Fare clic su "OK".

A questo punto, è possibile programmare semplicemente sul stored procedure come qualsiasi altro elemento nel modello.

Quindi, nella cartella "Controls" creare un nuovo controllo utente denominato AlsoPurchased. ascx.

Il markup per questo controllo riguarderà molto familiare al controllo PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La differenza principale consiste nel fatto che non memorizza nella cache l'output, perché il rendering dell'elemento sarà diverso per il prodotto.

ProductId sarà una "proprietà" del controllo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Nel gestore dell'evento PreRender del controllo, il fondo è a tre fattori.

1. Verificare che ProductID sia impostato.
2. Verificare se sono presenti prodotti acquistati con quello corrente.
3. Restituire alcuni elementi come determinato in #2.

Si noti quanto sia semplice chiamare il stored procedure tramite il modello.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Una volta determinato che è stato acquistato anche il metodo "purchased", è sufficiente associare il ripetitore ai risultati restituiti dalla query.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se non sono presenti elementi "acquistati anche", verranno semplicemente visualizzati altri elementi comuni del catalogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Per visualizzare gli elementi "acquistati anche", aprire la pagina ProductDetails. aspx e trascinare il controllo AlsoPurchased da Esplora soluzioni in modo che venga visualizzato in questa posizione nel markup.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

In questo modo viene creato un riferimento al controllo nella parte superiore della pagina ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Poiché il controllo utente AlsoPurchased richiede un numero ProductId, si imposterà la proprietà ProductID del controllo utilizzando un'istruzione eval sull'elemento del modello di dati corrente della pagina.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando si compila e si esegue ora e si passa a un prodotto, vengono visualizzati gli elementi "acquistati anche".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-6.md)
> [Successivo](tailspin-spyworks-part-8.md)
