---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: logica di business | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 5 aggiunge una logica di business.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630305"
---
# <a name="part-5-business-logic"></a>Parte 5: logica di business

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 5 aggiunge una logica di business.

## <a id="_Toc260221671"></a>Aggiunta di una logica di business

Si vuole che la nostra esperienza di acquisto sia disponibile ogni volta che qualcuno visita il sito Web. I visitatori saranno in grado di esplorare e aggiungere elementi al carrello acquisti anche se non sono registrati o connessi. Quando si è pronti per verificare, viene fornita la possibilità di eseguire l'autenticazione e, se non sono ancora membri, saranno in grado di creare un account.

Ciò significa che sarà necessario implementare la logica per convertire il carrello acquisti da uno stato anonimo a uno stato "utente registrato".

Creare una directory denominata "classi", quindi fare clic con il pulsante destro del mouse sulla cartella e creare un nuovo file "class" denominato MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Come indicato in precedenza, si estenderà la classe che implementa la pagina MyShoppingCart. aspx. questa operazione verrà eseguita usando. Costrutto "classe parziale" potente di NET.

La chiamata generata per il file MyShoppingCart.aspx.cf è simile alla seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Si noti l'uso della parola chiave "partial".

Il file di classe appena generato ha un aspetto simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Le implementazioni vengono unite aggiungendo la parola chiave partial anche a questo file.

Il nuovo file di classe ha ora un aspetto simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Il primo metodo che verrà aggiunto alla classe è il metodo "AddItem". Si tratta del metodo che verrà infine chiamato quando l'utente fa clic sui collegamenti "Aggiungi all'arte" nelle pagine elenco prodotti e dettagli prodotto.

Aggiungere quanto segue alle istruzioni using nella parte superiore della pagina.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E aggiungono questo metodo alla classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Si sta usando LINQ to Entities per verificare se l'elemento è già presente nel carrello. In tal caso, viene aggiornata la quantità dell'ordine dell'elemento. in caso contrario, viene creata una nuova voce per l'elemento selezionato

Per chiamare questo metodo, verrà implementata una pagina AddToCart. aspx che non solo è la classe di questo metodo, ma che ha quindi visualizzato la spesa corrente a = carrello dopo l'aggiunta dell'elemento.

Fare clic con il pulsante destro del mouse sul nome della soluzione in Esplora soluzioni e aggiungere e la nuova pagina denominata AddToCart. aspx, come è stato fatto in precedenza.

Sebbene sia possibile usare questa pagina per visualizzare risultati intermedi, ad esempio problemi di magazzino ridotti e così via, nell'implementazione di questa pagina non verrà effettivamente eseguito il rendering, bensì la logica di "aggiunta" e il reindirizzamento.

A tale scopo, verrà aggiunto il codice seguente alla pagina\_evento Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Si noti che viene recuperato il prodotto da aggiungere al carrello acquisti da un parametro QueryString e viene chiamato il metodo AddItem della classe.

Supponendo che non vengano rilevati errori, il controllo viene passato alla pagina SHoppingCart. aspx che verrà implementata completamente successivamente. Se si verifica un errore, viene generata un'eccezione.

Attualmente non è stato ancora implementato un gestore degli errori globale in modo che questa eccezione non venga gestita dall'applicazione, ma verrà rimediata a breve.

Si noti inoltre l'utilizzo dell'istruzione debug. Fail () (disponibile tramite `using System.Diagnostics;)`

Se l'applicazione è in esecuzione all'interno del debugger, questo metodo visualizzerà una finestra di dialogo dettagliata con informazioni sullo stato delle applicazioni insieme al messaggio di errore specificato.

Quando è in esecuzione nell'ambiente di produzione, l'istruzione debug. Fail () viene ignorata.

Si noterà che nel codice sopra una chiamata a un metodo nei nomi delle classi del carrello acquisti "GetShoppingCartId".

Aggiungere il codice per implementare il metodo come indicato di seguito.

Si noti che sono stati anche aggiunti i pulsanti di aggiornamento ed estrazione e un'etichetta in cui è possibile visualizzare il carrello "Total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

È ora possibile aggiungere elementi al carrello della spesa, ma la logica non è stata implementata per visualizzare il carrello dopo l'aggiunta di un prodotto.

Quindi, nella pagina MyShoppingCart. aspx verrà aggiunto un controllo EntityDataSource e un controllo GridVire come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chiamare il form nella finestra di progettazione in modo che sia possibile fare doppio clic sul pulsante Aggiorna carrello e generare il gestore dell'evento Click specificato nella dichiarazione nel markup.

Verranno implementati i dettagli in un secondo momento, ma questa operazione consente di compilare ed eseguire l'applicazione senza errori.

Quando si esegue l'applicazione e si aggiunge un elemento al carrello acquisti, questo verrà visualizzato.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Si noti che la visualizzazione della griglia "predefinita" è stata deviata implementando tre colonne personalizzate.

Il primo è un campo modificabile "associato" per la quantità:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

La successiva è una colonna "calcolata" che Visualizza il totale dell'elemento (il costo dell'elemento è la quantità da ordinare):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Infine, abbiamo una colonna personalizzata che contiene un controllo CheckBox che verrà usato dall'utente per indicare che l'elemento deve essere rimosso dal grafico degli acquisti.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Come si può notare, la riga Order Total è vuota, quindi verrà aggiunta una logica per calcolare il totale dell'ordine.

Per prima cosa verrà implementato un metodo "getTotal" per la classe MyShoppingCart.

Nel file MyShoppingCart.cs aggiungere il codice seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Quindi, nella pagina\_caricare il gestore eventi, è possibile chiamare il metodo getTotal. Allo stesso tempo, verrà aggiunto un test per verificare se il carrello acquisti è vuoto e modificare di conseguenza la visualizzazione, se è.

A questo punto, se il carrello acquisti è vuoto, si ottiene quanto segue:

![](tailspin-spyworks-part-5/_static/image4.jpg)

In caso contrario, viene visualizzato il totale.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Tuttavia, questa pagina non è ancora completa.

Sarà necessaria una logica aggiuntiva per ricalcolare il carrello della spesa rimuovendo gli elementi contrassegnati per la rimozione e determinando nuovi valori di quantità, perché alcuni potrebbero essere stati modificati nella griglia dall'utente.

Consente di aggiungere un metodo "RemoveItem" alla classe del carrello della spesa in MyShoppingCart.cs per gestire il caso in cui un utente contrassegna un elemento per la rimozione.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

A questo punto, ad un metodo è possibile gestire la circostanza quando un utente modifica semplicemente la qualità da ordinare in GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Con le funzionalità di base di rimozione e aggiornamento, è possibile implementare la logica che aggiorna effettivamente il carrello acquisti nel database. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Si noterà che questo metodo prevede due parametri. Uno è l'ID del carrello acquisti e l'altro è una matrice di oggetti di tipo definito dall'utente.

Per ridurre al minimo la dipendenza della logica sulle specifiche dell'interfaccia utente, è stata definita una struttura di dati che è possibile usare per passare gli elementi del carrello acquisti al codice senza che il metodo debba accedere direttamente al controllo GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Nel file MyShoppingCart.aspx.cs è possibile usare questa struttura nel gestore dell'evento click del pulsante Aggiorna, come indicato di seguito. Si noti che, oltre ad aggiornare il carrello, viene aggiornato anche il totale del carrello.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Si noti che con particolare interesse questa riga di codice:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () è una funzione di supporto speciale che verrà implementata in MyShoppingCart.aspx.cs, come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Questo consente di accedere in modo semplice ai valori degli elementi associati nel controllo GridView. Poiché il controllo CheckBox "Rimuovi elemento" non è associato, sarà possibile accedervi tramite il metodo FindControl ().

In questa fase dello sviluppo del progetto si è pronti per implementare il processo di estrazione.

Prima di procedere, è possibile usare Visual Studio per generare il database delle appartenenze e aggiungere un utente al repository di appartenenza.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-4.md)
> [Successivo](tailspin-spyworks-part-6.md)
