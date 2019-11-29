---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: creazione della pagina principale | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599981"
---
# <a name="part-7-creating-the-main-page"></a>Parte 7: creazione della pagina principale

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Creazione della pagina principale

In questa sezione verrà creata la pagina principale dell'applicazione. Questa pagina sarà più complessa della pagina di amministrazione, quindi verrà affrontata in diversi passaggi. Verranno visualizzate alcune tecniche più avanzate di Knockout. js. Ecco il layout di base della pagina:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products" include una matrice di prodotti.
- "Cart" include una matrice di prodotti con quantità. Se si fa clic su "Aggiungi al carrello", il carrello viene aggiornato.
- "Orders" include una matrice di ID degli ordini.
- "Details" include un dettaglio dell'ordine, ovvero una matrice di elementi (prodotti con quantità)

Si inizierà definendo un layout di base in HTML, senza data binding o script. Aprire il file views/Home/index. cshtml e sostituire tutto il contenuto con il codice seguente:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Successivamente, aggiungere una sezione script e creare un modello di visualizzazione vuoto:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

In base alla progettazione disegnata in precedenza, il modello di visualizzazione necessita di osservabili per prodotti, carrello, ordini e dettagli. Aggiungere le variabili seguenti all'oggetto `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Gli utenti possono aggiungere elementi dall'elenco dei prodotti al carrello e rimuovere gli elementi dal carrello. Per incapsulare queste funzioni, verrà creata un'altra classe del modello di visualizzazione che rappresenta un prodotto. Aggiungi il seguente codice a `AppViewModel`.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

La classe `ProductViewModel` contiene due funzioni utilizzate per spostare il prodotto da e verso il carrello: `addItemToCart` aggiunge un'unità del prodotto al carrello e `removeAllFromCart` rimuove tutte le quantità del prodotto.

Gli utenti possono selezionare un ordine esistente e ottenere i dettagli dell'ordine. Questa funzionalità verrà incapsulata in un altro modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Il `OrderDetailsViewModel` viene inizializzato con un ordine e recupera i dettagli dell'ordine inviando una richiesta AJAX al server.

Si noti inoltre la proprietà `total` nel `OrderDetailsViewModel`. Questa proprietà è un tipo speciale di osservabile definito [osservabile calcolato](http://knockoutjs.com/documentation/computedObservables.html). Come suggerisce il nome, un oggetto osservabile calcolato consente di eseguire il binding dei dati a&#8212;un valore calcolato, in questo caso, il costo totale dell'ordine.

Aggiungere quindi queste funzioni a `AppViewModel`:

- `resetCart` rimuove tutti gli elementi dal carrello.
- `getDetails` ottiene i dettagli per un ordine (eseguendo il push di un nuovo `OrderDetailsViewModel` nell'elenco `details`).
- `createOrder` crea un nuovo ordine e svuota il carrello.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Infine, inizializzare il modello di visualizzazione eseguendo richieste AJAX per i prodotti e gli ordini:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, si tratta di una grande quantità di codice, ma abbiamo creato una procedura dettagliata, quindi speriamo che la progettazione sia chiara. A questo punto è possibile aggiungere alcune associazioni knockout. js al codice HTML.

**Prodotti**

Ecco le associazioni per l'elenco di prodotti:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Viene eseguita l'iterazione della matrice Products e vengono visualizzati il nome e il prezzo. Il pulsante "Aggiungi a ordine" è visibile solo quando l'utente è connesso.

Il pulsante "Aggiungi all'ordine" chiama `addItemToCart` nell'istanza `ProductViewModel` per il prodotto. Viene illustrata una funzionalità interessante di Knockout. js: quando un modello di visualizzazione contiene altri modelli di visualizzazione, è possibile applicare le associazioni al modello interno. In questo esempio, le associazioni all'interno del `foreach` vengono applicate a ognuna delle istanze di `ProductViewModel`. Questo approccio è molto più pulito rispetto all'inserimento di tutte le funzionalità in un unico modello di visualizzazione.

**Carrello**

Ecco i binding per il carrello:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Viene eseguita l'iterazione sull'array del carrello e vengono visualizzati il nome, il prezzo e la quantità. Si noti che il collegamento "Rimuovi" e il pulsante "Crea ordine" sono associati alle funzioni di visualizzazione del modello.

**Ordini**

Ecco i binding per l'elenco ordini:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Viene eseguita l'iterazione degli ordini e viene visualizzato l'ID dell'ordine. L'evento click sul collegamento è associato alla funzione `getDetails`.

**Dettagli ordine**

Ecco le associazioni per i dettagli dell'ordine:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Scorre gli elementi nell'ordine e visualizza il prodotto, il prezzo e la quantità. Il div circostante è visibile solo se la matrice di dettagli contiene uno o più elementi.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stata creata un'applicazione che usa Entity Framework per comunicare con il database e API Web ASP.NET per fornire un'interfaccia pubblica sul livello dati. ASP.NET MVC 4 viene usato per eseguire il rendering delle pagine HTML ed knockout. js più jQuery per fornire interazioni dinamiche senza ricaricamenti di pagina.

Risorse aggiuntive:

- [Mappa del contenuto di accesso ai dati di ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro per sviluppatori Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-6.md)
