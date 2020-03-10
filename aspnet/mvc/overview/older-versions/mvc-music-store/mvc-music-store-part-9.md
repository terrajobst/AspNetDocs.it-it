---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registrazione ed estrazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 9 illustra la registrazione e l'estrazione.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559535"
---
# <a name="part-9-registration-and-checkout"></a>Parte 9: registrazione ed estrazione

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 9 illustra la registrazione e l'estrazione.

In questa sezione verrà creata una CheckoutController che raccoglierà l'indirizzo e le informazioni di pagamento del cliente. Prima di eseguire il checkout, è necessario che gli utenti si registrino con il sito, pertanto il controller richiederà l'autorizzazione.

Gli utenti accederanno al processo di estrazione dal carrello acquisti facendo clic sul pulsante "Estrai".

![](mvc-music-store-part-9/_static/image1.jpg)

Se l'utente non ha eseguito l'accesso, verrà richiesto di farlo.

![](mvc-music-store-part-9/_static/image1.png)

Al completamento dell'accesso, l'utente viene quindi visualizzato l'indirizzo e la visualizzazione dei pagamenti.

![](mvc-music-store-part-9/_static/image2.png)

Una volta compilato il modulo e inviato l'ordine, verrà visualizzata la schermata di conferma dell'ordine.

![](mvc-music-store-part-9/_static/image3.png)

Se si tenta di visualizzare un ordine inesistente o un ordine che non appartiene a, verrà visualizzata la visualizzazione degli errori.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrazione del carrello acquisti

Mentre il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante Checkout, sarà necessario registrarsi e accedere a. Gli utenti si aspettano che le informazioni del carrello acquisti vengano mantenute tra le visite, pertanto sarà necessario associare le informazioni del carrello acquisti a un utente quando completano la registrazione o l'accesso.

Questa operazione è molto semplice, in quanto la classe ShoppingCart dispone già di un metodo che associa tutti gli elementi nel carrello corrente a un nome utente. Quando un utente completa la registrazione o l'accesso, è sufficiente chiamare questo metodo.

Aprire la classe **AccountController** aggiunta durante la configurazione dell'appartenenza e dell'autorizzazione. Aggiungere un'istruzione using che fa riferimento a MvcMusicStore. Models, quindi aggiungere il metodo MigrateShoppingCart seguente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Modificare quindi l'azione post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Apportare la stessa modifica all'azione registra post, immediatamente dopo la creazione dell'account utente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

È ora possibile trasferire automaticamente un carrello acquisti anonimo a un account utente al termine della registrazione o dell'accesso.

## <a name="creating-the-checkoutcontroller"></a>Creazione di CheckoutController

Fare clic con il pulsante destro del mouse sulla cartella Controllers e aggiungere un nuovo controller al progetto denominato CheckoutController usando il modello di controller vuoto.

![](mvc-music-store-part-9/_static/image5.png)

Aggiungere prima di tutto l'attributo autorizza sopra la dichiarazione della classe controller per richiedere agli utenti di registrarsi prima del checkout:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: questa operazione è simile alla modifica apportata in precedenza a StoreManagerController, ma in questo caso l'attributo di autorizzazione richiede che l'utente sia in un ruolo di amministratore. Nel controller di checkout è necessario che l'utente sia connesso, ma non richiede che siano amministratori.*

Per motivi di semplicità, in questa esercitazione non verranno trattate le informazioni di pagamento. Al contrario, si consente agli utenti di consultare l'uso di un codice promozionale. Questo codice promozionale verrà archiviato usando una costante denominata promozione.

Come in StoreController, si dichiara un campo per includere un'istanza della classe MusicStoreEntities, denominata storeDB. Per poter usare la classe MusicStoreEntities, è necessario aggiungere un'istruzione using per lo spazio dei nomi MvcMusicStore. Models. Di seguito viene visualizzata la parte superiore del controller di estrazione.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Il CheckoutController avrà le seguenti azioni del controller:

**AddressAndPayment (Get Method)** visualizzerà un modulo per consentire all'utente di immettere le informazioni.

**AddressAndPayment (metodo post)** consente di convalidare l'input ed elaborare l'ordine.

Il **completamento** verrà visualizzato dopo che un utente ha completato il processo di estrazione. Questa visualizzazione includerà il numero di ordine dell'utente, come conferma.

Rinominare prima di tutto l'azione del controller di indice, che è stata generata quando è stato creato il controller, in AddressAndPayment. Questa azione del controller Visualizza solo il modulo di checkout, quindi non richiede informazioni sul modello.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Il metodo POST di AddressAndPayment seguirà lo stesso modello usato in StoreManagerController: tenterà di accettare l'invio del modulo e completerà l'ordine e visualizzerà nuovamente il modulo in caso di errore.

Dopo aver convalidato l'input del modulo soddisfa i requisiti di convalida per un ordine, si verificherà direttamente il valore del modulo Promozione. Supponendo che tutto sia corretto, le informazioni aggiornate vengono salvate con l'ordine, si indica all'oggetto ShoppingCart di completare il processo di ordine e si reindirizza all'azione completa.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Al termine del processo di checkout, gli utenti verranno reindirizzati all'azione completa del controller. Questa azione consente di eseguire un semplice controllo per verificare che l'ordine appartenga effettivamente all'utente connesso prima di visualizzare il numero di ordine come una conferma.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: la visualizzazione degli errori è stata creata automaticamente nella cartella/Views/Shared. quando è stato avviato il progetto.*

Il codice CheckoutController completo è il seguente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Aggiunta della visualizzazione AddressAndPayment

A questo punto, creare la visualizzazione AddressAndPayment. Fare clic con il pulsante destro del mouse su una delle azioni del controller AddressAndPayment e aggiungere una vista denominata AddressAndPayment che è fortemente tipizzata come ordine e usa il modello di modifica, come illustrato di seguito.

![](mvc-music-store-part-9/_static/image6.png)

Questa visualizzazione consente di usare due delle tecniche analizzate durante la creazione della vista StoreManagerEdit:

- Si userà HTML. EditorForModel () per visualizzare i campi del modulo per il modello Order
- Le regole di convalida vengono sfruttate usando una classe Order con attributi di convalida

Si inizierà con l'aggiornamento del codice del modulo per usare HTML. EditorForModel (), seguito da una casella di testo aggiuntiva per il codice promozionale. Di seguito è riportato il codice completo per la visualizzazione AddressAndPayment.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definizione delle regole di convalida per l'ordine

Ora che la visualizzazione è configurata, verranno configurate le regole di convalida per il modello Order come in precedenza per il modello di album. Fare clic con il pulsante destro del mouse sulla cartella Models e aggiungere una classe denominata Order. Oltre agli attributi di convalida usati in precedenza per l'album, si utilizzerà anche un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Se si tenta di inviare il modulo con informazioni mancanti o non valide, verrà visualizzato un messaggio di errore con la convalida lato client.

![](mvc-music-store-part-9/_static/image7.png)

È stata eseguita la maggior parte delle operazioni necessarie per il processo di checkout; Ci sono solo alcune probabilità e termina per terminare. È necessario aggiungere due visualizzazioni semplici ed è necessario prestare attenzione alla consegna delle informazioni del carrello durante il processo di accesso.

## <a name="adding-the-checkout-complete-view"></a>Aggiunta della visualizzazione completamento estrazione

La visualizzazione completamento estrazione è piuttosto semplice, in quanto deve solo visualizzare l'ID dell'ordine. Fare clic con il pulsante destro del mouse sull'azione completa controller e aggiungere una vista denominata completa che è fortemente tipizzata come int.

![](mvc-music-store-part-9/_static/image8.png)

A questo punto, verrà aggiornato il codice di visualizzazione per visualizzare l'ID dell'ordine, come mostrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aggiornamento della visualizzazione degli errori

Il modello predefinito include una visualizzazione degli errori nella cartella delle visualizzazioni condivise, in modo che possa essere riutilizzata altrove nel sito. Questa visualizzazione degli errori contiene un errore molto semplice e non usa il layout del sito, quindi verrà aggiornata.

Poiché si tratta di una pagina di errore generica, il contenuto è molto semplice. Si includerà un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente vuole riprovare a eseguire l'azione.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)
