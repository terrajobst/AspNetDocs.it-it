---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare elementi di dati e dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641113"
---
# <a name="display-data-items-and-details"></a>Visualizzazione di elementi di dati e dettagli

di [Erik Reitan](https://github.com/Erikre)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,7 e Microsoft Visual Studio 2017.

In questa esercitazione si apprenderà come visualizzare gli elementi di dati e i dettagli degli elementi di dati con ASP.NET Web Forms e Entity Framework Code First. Questa esercitazione si basa sull'esercitazione "UI and Navigation" precedente come parte della serie di esercitazioni su Wingtip Toy Store. Al termine dell'esercitazione, verranno visualizzati i prodotti nella pagina *Products. aspx* e i dettagli di un prodotto nella pagina *productDetails. aspx* .

## <a name="youll-learn-how-to"></a>Si imparerà a eseguire le operazioni seguenti:

- Aggiungere un controllo dati per visualizzare i prodotti dal database
- Connetti un controllo dati ai dati selezionati
- Aggiungere un controllo dati per visualizzare i dettagli del prodotto dal database
- Recuperare un valore dalla stringa di query e usare tale valore per limitare i dati recuperati dal database

### <a name="features-introduced-in-this-tutorial"></a>Funzionalità introdotte in questa esercitazione:

- Associazione di modelli
- Provider di valori

## <a name="add-a-data-control"></a>Aggiungere un controllo dati

È possibile utilizzare alcune opzioni diverse per associare i dati a un controllo server. Tra i più comuni:

* Aggiunta di un controllo origine dati
* Aggiunta di codice a mano
* Uso dell'associazione di modelli

### <a name="use-a-data-source-control-to-bind-data"></a>Usare un controllo origine dati per associare i dati

L'aggiunta di un controllo origine dati consente di collegare il controllo origine dati al controllo che Visualizza i dati. Con questo approccio, è possibile connettere i controlli lato server alle origini dati in modo dichiarativo, anziché a livello di codice.

### <a name="code-by-hand-to-bind-data"></a>Eseguire manualmente il codice per associare i dati

La codifica manuale prevede:

1. Lettura di un valore
2. Verifica della presenza di valori null
3. Conversione in un tipo appropriato
4. Verifica dell'esito positivo della conversione
5. Utilizzo del valore nella query 

Questo approccio consente di avere il controllo completo sulla logica di accesso ai dati.

### <a name="use-model-binding-to-bind-data"></a>Usare l'associazione di modelli per associare i dati

L'associazione di modelli consente di associare i risultati con una quantità di codice molto minore e offre la possibilità di riutilizzare la funzionalità nell'intera applicazione. Semplifica l'uso della logica di accesso ai dati incentrata sul codice, offrendo al tempo stesso un Framework completo per l'associazione dati.

## <a name="display-products"></a>Visualizza prodotti

In questa esercitazione si userà l'associazione di modelli per associare i dati. Per configurare un controllo dati per usare l'associazione di modelli per selezionare i dati, impostare la proprietà `SelectMethod` del controllo su un nome di metodo nel codice della pagina. Il controllo dei dati chiama il metodo nel momento appropriato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il metodo `DataBind`.

1. In **Esplora soluzioni**aprire *Production. aspx*.
2. Sostituire il markup esistente con il markup seguente:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Questo codice usa un controllo **ListView** denominato `productList` per visualizzare i prodotti.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con i modelli e gli stili è possibile definire il modo in cui il controllo **ListView** Visualizza i dati. È utile per i dati in qualsiasi struttura ripetuta. Sebbene questo esempio di **ListView** visualizzi semplicemente i dati del database, è anche possibile, senza codice, consentire agli utenti di modificare, inserire ed eliminare i dati e di ordinarli e di paginarli.

Impostando la proprietà `ItemType` nel controllo **ListView** , l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato. Come indicato nell'esercitazione precedente, è possibile selezionare Dettagli oggetto elemento con IntelliSense, ad esempio specificando la `ProductName`:

![Visualizzazione di elementi di dati e dettagli-IntelliSense](display_data_items_and_details/_static/image1.png)

Si usa anche l'associazione di modelli per specificare un valore `SelectMethod`. Questo valore (`GetProducts`) corrisponde al metodo che verrà aggiunto al code-behind per visualizzare i prodotti nel passaggio successivo.

### <a name="add-code-to-display-products"></a>Aggiungere codice per visualizzare i prodotti

In questo passaggio verrà aggiunto il codice per popolare il controllo **ListView** con i dati del prodotto del database. Il codice supporta la visualizzazione di tutti i prodotti e dei singoli prodotti di categoria.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *Production. aspx* , quindi scegliere **Visualizza codice**.
2. Sostituire il codice esistente nel file *ProductList.aspx.cs* con il codice seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Questo codice mostra il metodo `GetProducts` a cui fa riferimento la proprietà `ItemType` del controllo **ListView** nella pagina *Production. aspx* . Per limitare i risultati a una categoria di database specifica, il codice imposta il valore `categoryId` dal valore della stringa di query passato alla pagina *Production. aspx* quando si passa alla pagina *Production. aspx* . La classe `QueryStringAttribute` nello spazio dei nomi `System.Web.ModelBinding` viene utilizzata per recuperare il valore della variabile della stringa di query `id`. In questo modo si indica all'associazione di modelli di provare a associare un valore dalla stringa di query al parametro `categoryId` in fase di esecuzione.

Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati ai prodotti del database che corrispondono al valore `categoryId`. Ad esempio, se l'URL della pagina *Products. aspx* è il seguente:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.

Tutti i prodotti vengono visualizzati se non viene inclusa alcuna stringa di query quando viene chiamata la pagina *Production. aspx* .

Le origini dei valori per questi metodi sono denominate *provider* di valori (ad esempio *QueryString*) e gli attributi dei parametri che indicano il provider di valori da usare vengono definiti *attributi del provider di valori* , ad esempio `id`. ASP.NET include i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, i cookie, i valori dei moduli, i controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti di una categoria.

1. Premere **F5** in Visual Studio per eseguire l'applicazione.  
   Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .

2. Selezionare **automobili** dal menu di spostamento categoria prodotto.  
   La pagina *Production. aspx* Mostra solo i prodotti di categoria **Cars** . Più avanti in questa esercitazione verranno visualizzati i dettagli del prodotto.  

    ![Visualizzazione di elementi di dati e dettagli-automobili](display_data_items_and_details/_static/image2.png)

3. Selezionare **Products** dal menu di navigazione nella parte superiore.  
   Anche in questo caso, viene visualizzata la pagina *Production. aspx* , ma questa volta Mostra l'intero elenco dei prodotti.   

    ![Visualizzazione di elementi di dati e dettagli-prodotti](display_data_items_and_details/_static/image3.png)

4. Chiudere il browser e tornare a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Aggiungere un controllo dati per visualizzare i dettagli del prodotto

Successivamente, si modificherà il markup nella pagina *productDetails. aspx* aggiunta nell'esercitazione precedente per visualizzare informazioni specifiche sul prodotto.

1. In **Esplora soluzioni**aprire *productDetails. aspx*.

2. Sostituire il markup esistente con il markup seguente:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Questo codice usa un controllo **FormView** per visualizzare dettagli specifici del prodotto. Questo markup utilizza metodi come i metodi utilizzati per visualizzare i dati nella pagina *Production. aspx* . Il controllo **FormView** viene utilizzato per visualizzare un singolo record alla volta da un'origine dati. Quando si utilizza il controllo **FormView** , è possibile creare modelli per visualizzare e modificare i valori associati a dati. Questi modelli contengono controlli, espressioni di associazione e formattazione che definiscono l'aspetto e la funzionalità del modulo.

Per connettere il markup precedente al database è necessario codice aggiuntivo.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *productDetails. aspx* , quindi scegliere **Visualizza codice**.  
   Viene visualizzato il file *productDetails.aspx.cs* .

2. Sostituire il codice esistente con il codice seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Questo codice controlla la presenza di un valore della stringa di query "`productID`". Se viene trovato un valore di stringa di query valido, viene visualizzato il prodotto corrispondente. Se la stringa di query non viene trovata o il relativo valore non è valido, non viene visualizzato alcun prodotto.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

È ora possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato in base all'ID del prodotto.

1. Premere **F5** in Visual Studio per eseguire l'applicazione.  
   Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .

2. Selezionare **barche** dal menu di spostamento categoria.  
   Verrà visualizzata la pagina *Production. aspx* .

3. Selezionare **Paper Boat** dall'elenco dei prodotti.
   Viene visualizzata la pagina *productDetails. aspx* .

    ![Visualizzazione di elementi di dati e dettagli-prodotti](display_data_items_and_details/_static/image4.png)
    
4. Chiudere il browser.

## <a name="additional-resources"></a>Risorse aggiuntive

[Recupero e visualizzazione dei dati con l'associazione di modelli e Web Form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono stati aggiunti markup e codice per visualizzare i prodotti e i dettagli del prodotto. Sono stati appresi i controlli dati fortemente tipizzati, l'associazione di modelli e i provider di valori. Nell'esercitazione successiva verrà aggiunto un carrello acquisti all'applicazione di esempio Wingtip Toys. 

> [!div class="step-by-step"]
> [Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)
