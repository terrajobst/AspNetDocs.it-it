---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carrello acquisti con aggiornamenti Ajax | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 8 illustra il carrello della spesa con aggiornamenti AJAX.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539256"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: carrello acquisti con aggiornamenti Ajax

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 8 illustra il carrello della spesa con aggiornamenti AJAX.

Si consentirà agli utenti di inserire gli album nel carrello senza registrarli, ma sarà necessario registrarli come Guest per completare il checkout. Il processo di acquisti e di estrazione verrà suddiviso in due controller: un controller ShoppingCart che consente l'aggiunta anonima di elementi a un carrello e un controller di checkout che gestisce il processo di estrazione. Si inizierà con il carrello della spesa in questa sezione, quindi si creerà il processo di estrazione nella sezione seguente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Aggiunta delle classi del modello cart, Order e OrderDetail

Il carrello acquisti e i processi di estrazione useranno alcune nuove classi. Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe Cart (Cart.cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Questa classe è molto simile a quella usata finora, ad eccezione dell'attributo [key] per la proprietà RecordId. Gli elementi del carrello avranno un identificatore di stringa denominato CartID per consentire acquisti anonimi, ma la tabella include una chiave primaria integer denominata RecordId. Per convenzione, il codice Entity Framework prevede innanzitutto che la chiave primaria per una tabella denominata cart sia CartId o ID, ma è possibile eseguire facilmente l'override di tale codice tramite annotazioni o codice, se lo si desidera. Questo è un esempio di come è possibile usare le convenzioni semplici in Entity Framework Code-First quando si adattano a Microsoft, ma non è vincolato da loro.

Aggiungere quindi una classe Order (Order.cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Questa classe tiene traccia delle informazioni di riepilogo e di recapito per un ordine. Non verrà **ancora compilato**, perché include una proprietà di navigazione OrderDetails che dipende da una classe che non è ancora stata creata. Ora è possibile risolvere il problema aggiungendo una classe denominata OrderDetail.cs, aggiungendo il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Verrà apportato un ultimo aggiornamento alla classe MusicStoreEntities per includere DbSet che espongono le nuove classi del modello, includendo anche un DbSet&lt;Artist&gt;. La classe MusicStoreEntities aggiornata viene visualizzata come indicato di seguito.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gestione della logica di business del carrello acquisti

Successivamente, verrà creata la classe ShoppingCart nella cartella Models. Il modello ShoppingCart gestisce l'accesso ai dati alla tabella del carrello. Inoltre, la logica di business verrà gestita da per aggiungere e rimuovere elementi dal carrello acquisti.

Poiché non si desidera richiedere agli utenti di iscriversi a un account solo per aggiungere elementi al carrello acquisti, agli utenti viene assegnato un identificatore univoco temporaneo (usando un GUID o un identificatore univoco globale) quando accedono al carrello acquisti. Questo ID verrà archiviato usando la classe della sessione ASP.NET.

*Nota: la sessione ASP.NET è una soluzione ideale per archiviare le informazioni specifiche dell'utente che scadranno dopo l'uscita dal sito. Sebbene l'utilizzo improprio dello stato della sessione possa avere implicazioni sulle prestazioni nei siti più grandi, l'uso leggero funzionerà a scopo dimostrativo.*

La classe ShoppingCart espone i metodi seguenti:

**AddToCart** accetta un album come parametro e lo aggiunge al carrello dell'utente. Poiché la tabella del carrello tiene traccia della quantità per ogni album, include la logica per creare una nuova riga, se necessario, oppure incrementare solo la quantità se l'utente ha già ordinato una copia dell'album.

**RemoveFromCart** accetta un ID album e lo rimuove dal carrello dell'utente. Se l'utente ha una sola copia dell'album nel carrello, la riga viene rimossa.

**EmptyCart** rimuove tutti gli elementi dal carrello acquisti di un utente.

**GetCartItems** recupera un elenco di CartItems per la visualizzazione o l'elaborazione.

**GetCount** Recupera il numero totale di album di un utente nel carrello acquisti.

**GetTotal** calcola il costo totale di tutti gli elementi nel carrello.

**CreateOrder** converte il carrello acquisti in un ordine durante la fase di estrazione.

**GetCart** è un metodo statico che consente ai controller di ottenere un oggetto del carrello. Usa il metodo **GetCartId** per gestire la lettura del CartId dalla sessione dell'utente. Il metodo GetCartId richiede il HttpContextBase, in modo che possa leggere il CartId dell'utente dalla sessione dell'utente.

Di seguito è illustrata la **classe ShoppingCart**completa:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModel

Il controller del carrello acquisti dovrà comunicare alcune informazioni complesse con le visualizzazioni che non sono mappate agli oggetti modello. Non è necessario modificare i modelli in base alle visualizzazioni; Le classi del modello devono rappresentare il nostro dominio e non l'interfaccia utente. Una soluzione consiste nel passare le informazioni alle visualizzazioni usando la classe ViewBag, come è stato fatto con le informazioni a discesa di Store Manager, ma il passaggio di una grande quantità di informazioni tramite ViewBag diventa difficile da gestire.

Una soluzione a questo problema consiste nell'usare il modello *ViewModel* . Quando si usa questo modello, vengono create classi fortemente tipizzate ottimizzate per gli scenari di visualizzazione specifici e che espongono le proprietà per i valori dinamici o il contenuto necessari per i modelli di visualizzazione. Le classi controller possono quindi popolare e passare le classi ottimizzate per la visualizzazione al modello di visualizzazione da usare. In questo modo è possibile abilitare l'indipendenza dai tipi, il controllo in fase di compilazione e l'editor IntelliSense nei modelli di visualizzazione.

Verranno creati due modelli di visualizzazione da usare nel nostro controller del carrello della spesa: il ShoppingCartViewModel conterrà il contenuto del carrello acquisti dell'utente e ShoppingCartRemoveViewModel verrà usato per visualizzare le informazioni di conferma quando un utente rimuove qualcosa dal carrello.

Viene ora creata una nuova cartella ViewModels nella radice del progetto per organizzare le operazioni. Fare clic con il pulsante destro del mouse sul progetto e scegliere Aggiungi/nuova cartella.

![](mvc-music-store-part-8/_static/image1.jpg)

Denominare la cartella ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Successivamente, aggiungere la classe ShoppingCartViewModel nella cartella ViewModels. Dispone di due proprietà: un elenco di elementi del carrello e un valore decimale che contiene il prezzo totale per tutti gli elementi nel carrello.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Aggiungere ora ShoppingCartRemoveViewModel alla cartella ViewModels, con le quattro proprietà seguenti.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Il controller del carrello acquisti

Il controller del carrello acquisti ha tre scopi principali: l'aggiunta di elementi a un carrello, la rimozione di elementi dal carrello e la visualizzazione di elementi nel carrello. Utilizzerà le tre classi appena create: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart. Come in StoreController e StoreManagerController, si aggiungerà un campo per la detenuta di un'istanza di MusicStoreEntities.

Aggiungere al progetto un nuovo controller del carrello acquisti usando il modello di controller vuoto.

![](mvc-music-store-part-8/_static/image2.png)

Ecco il controller ShoppingCart completo. Le azioni index e Add controller dovrebbero avere un aspetto molto familiare. Le azioni del controller Remove e CartSummary gestiscono due casi speciali, che verranno illustrati nella sezione seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aggiornamenti AJAX con jQuery

Si creerà quindi una pagina di indice del carrello acquisti fortemente tipizzata per il ShoppingCartViewModel e si userà il modello di visualizzazione elenco usando lo stesso metodo precedente.

![](mvc-music-store-part-8/_static/image3.png)

Tuttavia, invece di usare un ActionLink HTML per rimuovere gli elementi dal carrello, si userà jQuery per "collegare" l'evento click per tutti i collegamenti in questa visualizzazione con la classe HTML RemoveLink. Anziché pubblicare il modulo, questo gestore dell'evento Click effettuerà semplicemente una richiamata AJAX all'azione del controller RemoveFromCart. RemoveFromCart restituisce un risultato serializzato JSON, che il callback jQuery analizza ed esegue quattro aggiornamenti rapidi alla pagina usando jQuery:

- 1. Rimuove l'album eliminato dall'elenco
- 2. Aggiorna il numero di carrelli nell'intestazione
- 3. Visualizza un messaggio di aggiornamento all'utente
- 4. Aggiorna il prezzo totale del carrello

Poiché lo scenario di rimozione è gestito da un callback Ajax nella visualizzazione dell'indice, non è necessaria una visualizzazione aggiuntiva per l'azione RemoveFromCart. Ecco il codice completo per la vista/ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Per eseguire il test, è necessario essere in grado di aggiungere elementi al carrello della spesa. La visualizzazione dei **Dettagli del negozio** verrà aggiornata in modo da includere un pulsante "Aggiungi al carrello". In questo articolo, possiamo includere alcune informazioni aggiuntive sull'album aggiunte dopo l'ultimo aggiornamento di questa visualizzazione: genere, artista, prezzo e album. Il codice di visualizzazione dettagli archivio aggiornato viene visualizzato come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

A questo punto, è possibile fare clic su Store e testare l'aggiunta e la rimozione di album da e verso il carrello della spesa. Eseguire l'applicazione e passare all'indice dello Store.

![](mvc-music-store-part-8/_static/image4.png)

Fare quindi clic su un genere per visualizzare un elenco di album.

![](mvc-music-store-part-8/_static/image5.png)

Facendo clic sul titolo di un album, viene ora visualizzata la visualizzazione dettagli album aggiornata, incluso il pulsante "Aggiungi al carrello".

![](mvc-music-store-part-8/_static/image6.png)

Facendo clic sul pulsante "Aggiungi al carrello" viene visualizzata la visualizzazione dell'indice del carrello acquisti con l'elenco di riepilogo della spesa.

![](mvc-music-store-part-8/_static/image7.png)

Dopo aver caricato il carrello della spesa, è possibile fare clic sul collegamento Rimuovi da carrello per visualizzare l'aggiornamento AJAX per il carrello della spesa.

![](mvc-music-store-part-8/_static/image8.png)

È stato creato un carrello acquisti funzionante che consente agli utenti non registrati di aggiungere elementi al carrello. Nella sezione seguente verrà consentito di registrare e completare il processo di estrazione.

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-7.md)
> [Successivo](mvc-music-store-part-9.md)
