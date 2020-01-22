---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carrello acquisti | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: d3b619ebd9448d30857ffbaf17fd245b1d54a662
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519297"
---
# <a name="shopping-cart"></a>Carrello acquisti

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

Questa esercitazione descrive la logica di business necessaria per aggiungere un carrello acquisti all'applicazione Web Forms di esempio Wingtip Toys ASP.NET. Questa esercitazione si basa sull'esercitazione precedente "visualizzare gli elementi di dati e i dettagli" e fa parte della serie di esercitazioni su Wingtip Toy Store. Al termine di questa esercitazione, gli utenti dell'app di esempio saranno in grado di aggiungere, rimuovere e modificare i prodotti nel carrello della spesa.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

1. Come creare un carrello acquisti per l'applicazione Web.
2. Come consentire agli utenti di aggiungere elementi al carrello acquisti.
3. Come aggiungere un controllo [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) per visualizzare i dettagli del carrello acquisti.
4. Come calcolare e visualizzare il totale dell'ordine.
5. Come rimuovere e aggiornare gli elementi nel carrello acquisti.
6. Come includere un contatore del carrello acquisti.

## <a name="code-features-in-this-tutorial"></a>Funzionalità di codice in questa esercitazione:

1. Entity Framework Code First
2. Annotazioni dei dati
3. Controlli dati fortemente tipizzati
4. Associazione di modelli

## <a name="creating-a-shopping-cart"></a>Creazione di un carrello acquisti

In precedenza in questa serie di esercitazioni è stato aggiunto il codice e le pagine per visualizzare i dati del prodotto da un database. In questa esercitazione verrà creato un carrello della spesa per gestire i prodotti che gli utenti sono interessati ad acquistare. Gli utenti potranno esplorare e aggiungere elementi al carrello della spesa anche se non sono registrati o connessi. Per gestire l'accesso al carrello acquisti, si assegnano agli utenti una `ID` univoca usando un identificatore univoco globale (GUID) quando l'utente accede al carrello acquisti per la prima volta. Questa `ID` verrà archiviata usando lo stato della sessione ASP.NET.

> [!NOTE] 
> 
> Lo stato della sessione ASP.NET è una posizione ideale per archiviare le informazioni specifiche dell'utente che scadranno dopo che l'utente ha lasciato il sito. Sebbene l'utilizzo improprio dello stato della sessione possa avere implicazioni sulle prestazioni nei siti più grandi, l'utilizzo leggero dello stato della sessione funziona bene a scopo dimostrativo. Il progetto di esempio Wingtip Toys Mostra come usare lo stato della sessione senza un provider esterno, in cui lo stato della sessione viene archiviato in-process nel server Web che ospita il sito. Per i siti più grandi che forniscono più istanze di un'applicazione o per siti che eseguono più istanze di un'applicazione su server diversi, è consigliabile utilizzare il **servizio cache di Microsoft Azure**. Questo servizio cache fornisce un servizio di memorizzazione nella cache distribuito esterno al sito Web e risolve il problema dell'utilizzo dello stato della sessione in-process. Per ulteriori informazioni, vedere [come utilizzare lo stato della sessione ASP.NET con siti Web di Microsoft Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).

### <a name="add-cartitem-as-a-model-class"></a>Aggiungere CartItem come classe modello

In precedenza in questa serie di esercitazioni è stato definito lo schema per la categoria e i dati del prodotto creando le classi `Category` e `Product` nella cartella *Models* . A questo punto, aggiungere una nuova classe per definire lo schema per il carrello acquisti. Più avanti in questa esercitazione verrà aggiunta una classe per gestire l'accesso ai dati alla tabella `CartItem`. Questa classe fornirà la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

1. Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** -&gt; **nuovo elemento**. 

    ![Carrello acquisti-nuovo elemento](shopping-cart/_static/image1.png)
2. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**. Selezionare il **codice**e quindi selezionare **classe**. 

    ![Carrello della spesa-finestra di dialogo Aggiungi nuovo elemento](shopping-cart/_static/image2.png)
3. Assegnare alla nuova classe il nome *CartItem.cs*.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe verrà visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La classe `CartItem` contiene lo schema che consente di definire ogni prodotto aggiunto da un utente al carrello acquisti. Questa classe è simile alle altre classi dello schema create in precedenza in questa serie di esercitazioni. Per convenzione, Entity Framework Code First prevede che la chiave primaria per la tabella `CartItem` sia `CartItemId` o `ID`. Tuttavia, il codice esegue l'override del comportamento predefinito utilizzando l'annotazione dati `[Key]` attributo. L'attributo `Key` della proprietà ItemId specifica che la proprietà `ItemID` è la chiave primaria.

La proprietà `CartId` specifica il `ID` dell'utente associato all'elemento da acquistare. Si aggiungerà il codice per creare l'utente `ID` quando l'utente accede al carrello della spesa. Questo `ID` verrà archiviato anche come variabile di sessione ASP.NET.

### <a name="update-the-product-context"></a>Aggiornare il contesto del prodotto

Oltre ad aggiungere la classe `CartItem`, sarà necessario aggiornare la classe del contesto di database che gestisce le classi di entità e che fornisce l'accesso ai dati al database. A tale scopo, verrà aggiunta la classe del modello di `CartItem` appena creata alla classe `ProductContext`.

1. In **Esplora soluzioni**individuare e aprire il file *ProductContext.cs* nella cartella *Models* .
2. Aggiungere il codice evidenziato al file *ProductContext.cs* nel modo seguente:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Come indicato in precedenza in questa serie di esercitazioni, il codice nel file *ProductContext.cs* aggiunge lo spazio dei nomi `System.Data.Entity` in modo da avere accesso a tutte le funzionalità di base del Entity Framework. Questa funzionalità include la funzionalità per eseguire query e inserire, aggiornare ed eliminare dati mediante l'utilizzo di oggetti fortemente tipizzati. La classe `ProductContext` aggiunge l'accesso alla classe del modello `CartItem` appena aggiunta.

### <a name="managing-the-shopping-cart-business-logic"></a>Gestione della logica di business del carrello acquisti

Successivamente, si creerà la classe `ShoppingCart` in una nuova cartella *logica* . La classe `ShoppingCart` gestisce l'accesso ai dati nella tabella `CartItem`. La classe includerà inoltre la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

La logica del carrello acquisti che si aggiungerà conterrà la funzionalità per gestire le azioni seguenti:

1. Aggiunta di elementi al carrello acquisti
2. Rimozione di elementi dal carrello acquisti
3. Recupero dell'ID carrello acquisti
4. Recupero di elementi dal carrello acquisti
5. Totale della quantità di tutti gli elementi del carrello acquisti
6. Aggiornamento dei dati del carrello acquisti

Una pagina del carrello acquisti (*ShoppingCart. aspx*) e la classe del carrello acquisti verranno utilizzate insieme per accedere ai dati del carrello acquisti. Nella pagina carrello acquisti verranno visualizzati tutti gli elementi che l'utente aggiunge al carrello acquisti. Oltre alla pagina e alla classe del carrello acquisti, è possibile creare una pagina (*AddToCart. aspx*) per aggiungere prodotti al carrello acquisti. Si aggiungerà anche codice alla pagina *Production. aspx* e alla pagina *productDetails. aspx* che fornirà un collegamento alla pagina *AddToCart. aspx* , in modo che l'utente possa aggiungere prodotti al carrello acquisti.

Il diagramma seguente illustra il processo di base che si verifica quando l'utente aggiunge un prodotto al carrello acquisti.

![Carrello della spesa-aggiunta al carrello](shopping-cart/_static/image3.png)

Quando l'utente fa clic sul collegamento **Aggiungi al carrello** nella pagina *Production. aspx* o nella pagina *productDetails. aspx* , l'applicazione passa alla pagina *AddToCart. aspx* e quindi automaticamente alla pagina *ShoppingCart. aspx* . La pagina *AddToCart. aspx* consente di aggiungere il prodotto Select al carrello della spesa chiamando un metodo nella classe ShoppingCart. Nella pagina *ShoppingCart. aspx* vengono visualizzati i prodotti aggiunti al carrello acquisti.

#### <a name="creating-the-shopping-cart-class"></a>Creazione della classe del carrello acquisti

La classe `ShoppingCart` verrà aggiunta a una cartella separata nell'applicazione, in modo che vi sia una netta distinzione tra il modello (cartella dei modelli), le pagine (cartella radice) e la logica (cartella logica).

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WingtipToys**e scegliere **Aggiungi**-&gt;**nuova cartella**. Assegnare un nome alla nuova *logica*della cartella.
2. Fare clic con il pulsante destro del mouse sulla cartella *logica* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.
3. Aggiungere un nuovo file di classe denominato *ShoppingCartActions.cs*.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Il metodo `AddToCart` consente l'inclusione di singoli prodotti nel carrello acquisti in base al `ID`del prodotto. Il prodotto viene aggiunto al carrello o, se il carrello contiene già un elemento per quel prodotto, la quantità viene incrementata.

Il metodo `GetCartId` restituisce il carrello `ID` per l'utente. Il `ID` del carrello viene usato per tenere traccia degli elementi che un utente ha nel carrello acquisti. Se l'utente non dispone di un `ID`del carrello esistente, viene creata una nuova `ID` del carrello. Se l'utente ha effettuato l'accesso come utente registrato, il `ID` del carrello viene impostato sul nome utente. Tuttavia, se l'utente non è connesso, il `ID` del carrello viene impostato su un valore univoco (GUID). Un GUID garantisce che venga creato un solo carrello per ogni utente, in base alla sessione.

Il metodo `GetCartItems` restituisce un elenco di elementi del carrello acquisti per l'utente. Più avanti in questa esercitazione si noterà che l'associazione di modelli viene usata per visualizzare gli elementi del carrello nel carrello acquisti usando il metodo `GetCartItems`.

### <a name="creating-the-add-to-cart-functionality"></a>Creazione della funzionalità di aggiunta al carrello

Come indicato in precedenza, si creerà una pagina di elaborazione denominata *AddToCart. aspx* che verrà usata per aggiungere nuovi prodotti al carrello degli acquisti dell'utente. Questa pagina chiamerà il metodo `AddToCart` nella classe `ShoppingCart` appena creata. Nella pagina *AddToCart. aspx* si prevede che venga passato un `ID` del prodotto. Questo `ID` di prodotto verrà usato quando si chiama il metodo `AddToCart` nella classe `ShoppingCart`.

> [!NOTE] 
> 
> Si modificherà il code-behind (*AddToCart.aspx.cs*) per questa pagina, non l'interfaccia utente della pagina (*AddToCart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>Per creare la funzionalità di aggiunta al carrello:

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WingtipToys**, quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina (Web Form) standard all'applicazione denominata *AddToCart. aspx*. 

    ![Carrello acquisti-Aggiungi modulo Web](shopping-cart/_static/image4.png)
3. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina *AddToCart. aspx* , quindi scegliere **Visualizza codice**. Il file code-behind *AddToCart.aspx.cs* viene aperto nell'editor.
4. Sostituire il codice esistente nel code-behind *AddToCart.aspx.cs* con quanto segue:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Quando viene caricata la pagina *AddToCart. aspx* , il `ID` del prodotto viene recuperato dalla stringa di query. Viene quindi creata un'istanza della classe del carrello acquisti che verrà utilizzata per chiamare il metodo `AddToCart` aggiunto in precedenza in questa esercitazione. Il metodo `AddToCart`, contenuto nel file *ShoppingCartActions.cs* , include la logica per aggiungere il prodotto selezionato al carrello acquisti oppure incrementare la quantità di prodotto del prodotto selezionato. Se il prodotto non è stato aggiunto al carrello acquisti, il prodotto viene aggiunto alla tabella `CartItem` del database. Se il prodotto è già stato aggiunto al carrello acquisti e l'utente aggiunge un elemento aggiuntivo dello stesso prodotto, la quantità di prodotto viene incrementata nella tabella `CartItem`. Infine, la pagina reindirizza nuovamente alla pagina *ShoppingCart. aspx* che verrà aggiunta nel passaggio successivo, in cui l'utente visualizza un elenco aggiornato di elementi nel carrello.

Come indicato in precedenza, un utente `ID` viene usato per identificare i prodotti associati a un utente specifico. Questa `ID` viene aggiunta a una riga nella tabella `CartItem` ogni volta che l'utente aggiunge un prodotto al carrello acquisti.

### <a name="creating-the-shopping-cart-ui"></a>Creazione dell'interfaccia utente del carrello acquisti

Nella pagina *ShoppingCart. aspx* vengono visualizzati i prodotti che l'utente ha aggiunto al carrello acquisti. Consente inoltre di aggiungere, rimuovere e aggiornare elementi nel carrello acquisti.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su **WingtipToys**, quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina (Web Form) che includa una pagina master selezionando **Web Form tramite la pagina master**. Denominare la nuova pagina *ShoppingCart. aspx*.
3. Selezionare **site. master** per alleghi la pagina master alla pagina *. aspx* appena creata.
4. Nella pagina *ShoppingCart. aspx* sostituire il markup esistente con il markup seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

La pagina *ShoppingCart. aspx* include un controllo **GridView** denominato `CartList`. Questo controllo Usa l'associazione di modelli per associare i dati del carrello acquisti dal database al controllo **GridView** . Quando si imposta la proprietà `ItemType` del controllo **GridView** , l'espressione di associazione dati `Item` è disponibile nel markup del controllo e il controllo diventa fortemente tipizzato. Come indicato in precedenza in questa serie di esercitazioni, è possibile selezionare i dettagli dell'oggetto `Item` usando IntelliSense. Per configurare un controllo dati per l'utilizzo dell'associazione di modelli per selezionare i dati, impostare la proprietà `SelectMethod` del controllo. Nel markup precedente si imposta la `SelectMethod` per usare il metodo GetShoppingCartItems, che restituisce un elenco di `CartItem` oggetti. Il controllo dati **GridView** chiama il metodo nel momento appropriato del ciclo di vita della pagina e associa automaticamente i dati restituiti. È ancora necessario aggiungere il metodo `GetShoppingCartItems`.

#### <a name="retrieving-the-shopping-cart-items"></a>Recupero degli elementi del carrello acquisti

Successivamente, aggiungere il codice al code-behind *ShoppingCart.aspx.cs* per recuperare e popolare l'interfaccia utente del carrello acquisti.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina *ShoppingCart. aspx* , quindi scegliere **Visualizza codice**. Il file code-behind *ShoppingCart.aspx.cs* viene aperto nell'editor.
2. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Come indicato in precedenza, il controllo dati `GridView` chiama il metodo `GetShoppingCartItems` al momento appropriato nel ciclo di vita della pagina e associa automaticamente i dati restituiti. Il metodo `GetShoppingCartItems` crea un'istanza dell'oggetto `ShoppingCartActions`. Quindi, il codice usa tale istanza per restituire gli elementi nel carrello chiamando il metodo `GetCartItems`.

### <a name="adding-products-to-the-shopping-cart"></a>Aggiunta di prodotti al carrello acquisti

Quando viene visualizzata la pagina *Production. aspx* o *productDetails. aspx* , l'utente sarà in grado di aggiungere il prodotto al carrello acquisti usando un collegamento. Quando si fa clic sul collegamento, l'applicazione passa alla pagina di elaborazione denominata *AddToCart. aspx*. La pagina *AddToCart. aspx* chiamerà il metodo `AddToCart` nella classe `ShoppingCart` aggiunta in precedenza in questa esercitazione.

A questo punto, verrà aggiunto un collegamento **Aggiungi al carrello** alla pagina *Production. aspx* e alla pagina *productDetails. aspx* . Questo collegamento includerà il `ID` del prodotto recuperato dal database.

1. In **Esplora soluzioni**individuare e aprire la pagina denominata *Production. aspx*.
2. Aggiungere il markup evidenziato in giallo alla pagina *Production. aspx* in modo che l'intera pagina appaia come segue:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Test del carrello acquisti

Eseguire l'applicazione per vedere come aggiungere i prodotti al carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Dopo che il progetto ha ricreato il database, il browser si aprirà e visualizzerà la pagina *default. aspx* .
2. Selezionare **automobili** dal menu di spostamento categoria.  
 Viene visualizzata la pagina *Production. aspx* che mostra solo i prodotti inclusi nella categoria "Cars". 

    ![Carrello acquisti-automobili](shopping-cart/_static/image5.png)
3. Fare clic sul collegamento **Aggiungi al carrello** accanto al primo prodotto elencato (auto convertibile).   
 Viene visualizzata la pagina *ShoppingCart. aspx* , che mostra la selezione nel carrello della spesa. 

    ![Carrello acquisti-carrello](shopping-cart/_static/image6.png)
4. Per visualizzare prodotti aggiuntivi, selezionare **piani** dal menu di spostamento categoria.
5. Fare clic sul collegamento **Aggiungi al carrello** accanto al primo prodotto elencato.  
 Viene visualizzata la pagina *ShoppingCart. aspx* con l'elemento aggiuntivo.
6. Chiudere il browser.

### <a name="calculating-and-displaying-the-order-total"></a>Calcolo e visualizzazione del totale dell'ordine

Oltre ad aggiungere prodotti al carrello della spesa, si aggiungerà un metodo di `GetTotal` alla classe `ShoppingCart` e si visualizzerà l'importo totale dell'ordine nella pagina del carrello acquisti.

1. In **Esplora soluzioni**aprire il file *ShoppingCartActions.cs* nella cartella della *logica* .
2. Aggiungere il seguente metodo di `GetTotal` evidenziato in giallo alla classe `ShoppingCart`, in modo che la classe appaia come segue:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

In primo luogo, il metodo `GetTotal` Ottiene l'ID del carrello acquisti per l'utente. Il metodo ottiene quindi il totale del carrello moltiplicando il prezzo del prodotto in base alla quantità di prodotto per ogni prodotto elencato nel carrello.

> [!NOTE] 
> 
> Il codice precedente usa il tipo Nullable "`int?`". I tipi nullable possono rappresentare tutti i valori di un tipo sottostante e anche come valore null. Per ulteriori informazioni, vedere [utilizzo dei tipi nullable](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).

### <a name="modify-the-shopping-cart-display"></a>Modificare la visualizzazione del carrello acquisti

Successivamente, si modificherà il codice per la pagina *ShoppingCart. aspx* per chiamare il metodo `GetTotal` e visualizzare il totale nella pagina *ShoppingCart. aspx* quando viene caricata la pagina.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina *ShoppingCart. aspx* e selezionare **Visualizza codice**.
2. Nel file *ShoppingCart.aspx.cs* aggiornare il gestore `Page_Load` aggiungendo il codice seguente evidenziato in giallo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Quando viene caricata la pagina *ShoppingCart. aspx* , carica l'oggetto carrello acquisti e recupera il totale del carrello acquisti chiamando il metodo `GetTotal` della classe `ShoppingCart`. Se il carrello acquisti è vuoto, viene visualizzato un messaggio a tale effetto.

### <a name="testing-the-shopping-cart-total"></a>Test del totale del carrello acquisti

Eseguire ora l'applicazione per vedere come non solo aggiungere un prodotto al carrello acquisti, ma è possibile vedere il totale del carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Si aprirà il browser e verrà visualizzata la pagina *Default.aspx* .
2. Selezionare **automobili** dal menu di spostamento categoria.
3. Fare clic sul collegamento **Aggiungi al carrello** accanto al primo prodotto.   
 La pagina *ShoppingCart. aspx* viene visualizzata con il totale dell'ordine. 

    ![Carrello acquisti-totale carrello](shopping-cart/_static/image7.png)
4. Aggiungere altri prodotti, ad esempio un piano, al carrello.
5. Viene visualizzata la pagina *ShoppingCart. aspx* con un totale aggiornato per tutti i prodotti aggiunti. 

    ![Carrello acquisti-più prodotti](shopping-cart/_static/image8.png)
6. Arrestare l'app in esecuzione chiudendo la finestra del browser.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Aggiunta di pulsanti di aggiornamento ed estrazione al carrello acquisti

Per consentire agli utenti di modificare il carrello acquisti, è necessario aggiungere un pulsante di **aggiornamento** e un pulsante di **estrazione** alla pagina del carrello acquisti. Il pulsante **Estrai** non viene usato più avanti in questa serie di esercitazioni.

1. In **Esplora soluzioni**aprire la pagina *ShoppingCart. aspx* nella radice del progetto di applicazione Web.
2. Per aggiungere il pulsante **Aggiorna** e il pulsante **checkout** alla pagina *ShoppingCart. aspx* , aggiungere il markup evidenziato in giallo al markup esistente, come illustrato nel codice seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quando l'utente fa clic sul pulsante **Aggiorna** , viene chiamato il gestore dell'evento `UpdateBtn_Click`. Questo gestore eventi chiamerà il codice che verrà aggiunto nel passaggio successivo.

Successivamente, è possibile aggiornare il codice contenuto nel file *ShoppingCart.aspx.cs* per scorrere in ciclo gli elementi del carrello e chiamare i metodi `RemoveItem` e `UpdateItem`.

1. In **Esplora soluzioni**aprire il file *ShoppingCart.aspx.cs* nella radice del progetto di applicazione Web.
2. Aggiungere le sezioni di codice seguenti evidenziate in giallo per il file *ShoppingCart.aspx.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quando l'utente fa clic sul pulsante **Aggiorna** nella pagina *ShoppingCart. aspx* , viene chiamato il metodo UpdateCartItems. Il metodo UpdateCartItems ottiene i valori aggiornati per ogni elemento nel carrello acquisti. Quindi, il metodo UpdateCartItems chiama il metodo `UpdateShoppingCartDatabase` (aggiunto e illustrato nel passaggio successivo) per aggiungere o rimuovere elementi dal carrello della spesa. Dopo che il database è stato aggiornato per riflettere gli aggiornamenti al carrello della spesa, il controllo **GridView** viene aggiornato nella pagina del carrello della spesa chiamando il metodo `DataBind` per **GridView**. Inoltre, l'importo totale dell'ordine nella pagina del carrello acquisti viene aggiornato in modo da riflettere l'elenco aggiornato di elementi.

### <a name="updating-and-removing-shopping-cart-items"></a>Aggiornamento e rimozione degli elementi del carrello acquisti

Nella pagina *ShoppingCart. aspx* è possibile vedere che sono stati aggiunti controlli per l'aggiornamento della quantità di un elemento e la rimozione di un elemento. A questo punto, aggiungere il codice che renderà il lavoro di questi controlli.

1. In **Esplora soluzioni**aprire il file *ShoppingCartActions.cs* nella cartella della *logica* .
2. Aggiungere il codice seguente evidenziato in giallo al file della classe *ShoppingCartActions.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Il metodo `UpdateShoppingCartDatabase`, chiamato dal metodo `UpdateCartItems` nella pagina *ShoppingCart.aspx.cs* , contiene la logica per aggiornare o rimuovere elementi dal carrello acquisti. Il metodo `UpdateShoppingCartDatabase` scorre tutte le righe all'interno dell'elenco del carrello acquisti. Se un elemento del carrello acquisti è stato contrassegnato per la rimozione o la quantità è minore di uno, viene chiamato il metodo `RemoveItem`. In caso contrario, viene verificata la disponibilità di aggiornamenti nell'elemento del carrello acquisti quando viene chiamato il metodo `UpdateItem`. Dopo che l'elemento del carrello acquisti è stato rimosso o aggiornato, le modifiche apportate al database vengono salvate.

La struttura `ShoppingCartUpdates` viene utilizzata per conservare tutti gli elementi del carrello acquisti. Il metodo `UpdateShoppingCartDatabase` usa la struttura `ShoppingCartUpdates` per determinare se uno degli elementi deve essere aggiornato o rimosso.

Nell'esercitazione successiva si userà il metodo `EmptyCart` per cancellare il carrello acquisti dopo aver acquistato i prodotti. Per il momento verrà usato il metodo `GetCount` appena aggiunto al file *ShoppingCartActions.cs* per determinare il numero di elementi presenti nel carrello della spesa.

### <a name="adding-a-shopping-cart-counter"></a>Aggiunta di un contatore carrello acquisti

Per consentire all'utente di visualizzare il numero totale di elementi nel carrello acquisti, viene aggiunto un contatore alla pagina *site. master* . Questo contatore fungerà anche da collegamento al carrello della spesa.

1. In **Esplora soluzioni**aprire la pagina *site. master* .
2. Modificare il markup aggiungendo il collegamento del contatore del carrello acquisti come illustrato in giallo alla sezione di navigazione, in modo che venga visualizzato come segue:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Aggiornare quindi il code-behind del file *site.master.cs* aggiungendo il codice evidenziato in giallo come indicato di seguito:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Prima che venga eseguito il rendering della pagina in formato HTML, viene generato l'evento `Page_PreRender`. Nel gestore `Page_PreRender` il conteggio totale del carrello acquisti viene determinato chiamando il metodo `GetCount`. Il valore restituito viene aggiunto all'intervallo `cartCount` incluso nel markup della pagina *site. master* . Il `<span>` tag consente di eseguire correttamente il rendering degli elementi interni. Quando viene visualizzata una pagina del sito, viene visualizzato il totale del carrello acquisti. L'utente può anche fare clic sul totale del carrello acquisti per visualizzare il carrello.

## <a name="testing-the-completed-shopping-cart"></a>Test del carrello acquisti completato

È possibile eseguire ora l'applicazione per vedere come è possibile aggiungere, eliminare e aggiornare gli elementi nel carrello acquisti. Il totale del carrello acquisti riflette il costo totale di tutti gli elementi nel carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .
2. Selezionare **automobili** dal menu di spostamento categoria.
3. Fare clic sul collegamento **Aggiungi al carrello** accanto al primo prodotto.   
 La pagina *ShoppingCart. aspx* viene visualizzata con il totale dell'ordine.
4. Selezionare **piani** dal menu di spostamento categoria.
5. Fare clic sul collegamento **Aggiungi al carrello** accanto al primo prodotto.
6. Impostare la quantità del primo elemento del carrello acquisti su 3 e selezionare la casella di controllo **Rimuovi elemento** del secondo elemento.<a id="a"></a>
7. Fare clic sul pulsante **Aggiorna** per aggiornare la pagina del carrello acquisti e visualizzare il nuovo totale degli ordini. 

    ![Carrello acquisti-aggiornamento carrello](shopping-cart/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato creato un carrello acquisti per l'applicazione di esempio Wingtip Toys Web Forms. Durante questa esercitazione sono stati usati Entity Framework Code First, le annotazioni dei dati, i controlli dati fortemente tipizzati e l'associazione di modelli.

Il carrello acquisti supporta l'aggiunta, l'eliminazione e l'aggiornamento degli elementi selezionati dall'utente per l'acquisto. Oltre a implementare la funzionalità del carrello acquisti, si è appreso come visualizzare gli elementi del carrello acquisti in un controllo **GridView** e calcolare il totale dell'ordine.

Per comprendere il funzionamento della funzionalità descritta in un'applicazione aziendale reale, è possibile visualizzare l'esempio del carrello degli acquisti di eCommerce open source basato su [nopCommerce](https://github.com/nopSolutions/nopCommerce) -ASP.NET. Originariamente, era basata su Web Forms e negli anni passati a MVC e ora a ASP.NET Core.

## <a name="addition-information"></a>Informazioni aggiuntive

[Cenni preliminari sullo stato della sessione ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Precedente](display_data_items_and_details.md)
> [Successivo](checkout-and-payment-with-paypal.md)
