---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevenzione degli attacchi injection JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Impedisci l'esecuzione degli attacchi intrusivi in JavaScript e degli attacchi tramite script intersito. In questa esercitazione, Stephen Walther spiega come è possibile...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: dfe09085f26c62c566649bc6f570aa25367a0f07
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594796"
---
# <a name="preventing-javascript-injection-attacks-vb"></a>Prevenzione degli attacchi injection JavaScript (VB)

di [Stephen Walther](https://github.com/StephenWalther)

[Scaricare PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Impedisci l'esecuzione degli attacchi intrusivi in JavaScript e degli attacchi tramite script intersito. In questa esercitazione Stephen Walther spiega come è possibile sconfiggere facilmente questi tipi di attacchi mediante la codifica HTML del contenuto.

L'obiettivo di questa esercitazione è spiegare come è possibile impedire gli attacchi intrusivi JavaScript nelle applicazioni MVC ASP.NET. Questa esercitazione illustra due approcci per difendere il sito Web da un attacco JavaScript injection. Si apprenderà come impedire gli attacchi injection JavaScript codificando i dati visualizzati. Si apprenderà anche come impedire gli attacchi injection JavaScript codificando i dati accettati.

## <a name="what-is-a-javascript-injection-attack"></a>Che cos'è un attacco JavaScript Injection?

Ogni volta che si accetta l'input dell'utente e si visualizza nuovamente l'input dell'utente, si apre il sito Web per gli attacchi intrusivi in JavaScript. Si esaminerà un'applicazione concreta che è aperta ad attacchi JavaScript injection.

Si supponga di aver creato un sito Web per i commenti e suggerimenti dei clienti (vedere la figura 1). I clienti possono visitare il sito Web e immettere commenti e suggerimenti sulla propria esperienza usando i prodotti. Quando un cliente invia il proprio feedback, i commenti vengono visualizzati nella pagina commenti e suggerimenti.

[Sito Web ![commenti e suggerimenti dei clienti](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: sito Web di commenti e suggerimenti dei clienti ([fare clic per visualizzare l'immagine con dimensioni complete](preventing-javascript-injection-attacks-vb/_static/image3.png))

Il sito Web di commenti e suggerimenti dei clienti USA il `controller` nel listato 1. Questo `controller` contiene due azioni denominate `Index()` e `Create()`.

**Listato 1-`HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Il metodo `Index()` Visualizza la visualizzazione `Index`. Questo metodo passa tutti i commenti dei clienti precedenti alla visualizzazione `Index` recuperando il feedback dal database (usando una query di LINQ to SQL).

Il metodo `Create()` crea un nuovo elemento di feedback e lo aggiunge al database. Il messaggio immesso dal cliente nel form viene passato al metodo `Create()` nel parametro message. Viene creato un elemento di feedback e il messaggio viene assegnato alla proprietà `Message` dell'elemento feedback. L'elemento feedback viene inviato al database con la chiamata al metodo `DataContext.SubmitChanges()`. Infine, il visitatore viene reindirizzato di nuovo alla visualizzazione `Index` in cui vengono visualizzati tutti i commenti.

La visualizzazione `Index` è inclusa nel listato 2.

**Listato 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

La visualizzazione `Index` include due sezioni. La sezione superiore contiene il modulo effettivo per i commenti dei clienti. La sezione inferiore contiene un oggetto per. Ogni ciclo esegue il ciclo di tutti gli elementi di commenti e suggerimenti dei clienti precedenti e visualizza le proprietà EntryDate e Message per ogni elemento di feedback.

Il sito Web di commenti e suggerimenti dei clienti è un sito Web semplice. Sfortunatamente, il sito Web è aperto ad attacchi JavaScript injection.

Si supponga di immettere il testo seguente nel modulo feedback del cliente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Questo testo rappresenta uno script JavaScript che visualizza una finestra di messaggio di avviso. Dopo che qualcuno ha inviato questo script al modulo feedback, il messaggio <em>Boo!</em> verrà visualizzato ogni volta che qualcuno visita il sito Web di commenti e suggerimenti dei clienti in futuro (vedere la figura 2).

[![injection JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: inserimento di JavaScript ([fare clic per visualizzare l'immagine a dimensione intera](preventing-javascript-injection-attacks-vb/_static/image6.png))

A questo punto, la risposta iniziale a attacchi JavaScript Injection potrebbe essere apatia. Si potrebbe pensare che gli attacchi di tipo JavaScript Injection siano semplicemente un tipo di attacco di tipo *devisore* . Si potrebbe ritenere che nessuno possa eseguire alcuna operazione realmente male eseguendo un attacco JavaScript injection.

Sfortunatamente, un pirata informatico può eseguire alcune operazioni in realtà, inserendo JavaScript in un sito Web. È possibile usare un attacco JavaScript Injection per eseguire un attacco XSS (cross-site scripting). In un attacco di scripting tra siti, si rubano informazioni riservate sull'utente e si inviano le informazioni a un altro sito Web.

Un pirata informatico può ad esempio usare un attacco JavaScript Injection per rubare i valori dei cookie del browser da altri utenti. Se le informazioni riservate, ad esempio le password, i numeri di carta di credito o i numeri di previdenza sociale, vengono archiviate nei cookie del browser, un hacker può usare un attacco JavaScript Injection per rubare queste informazioni. In alternativa, se un utente immette informazioni riservate in un campo del modulo contenuto in una pagina che è stata compromessa da un attacco JavaScript, l'hacker può usare il codice JavaScript inserito per acquisire i dati del modulo e inviarli a un altro sito Web.

*Si è preoccupati*. Eseguire seriamente attacchi JavaScript Injection e proteggere le informazioni riservate dell'utente. Nelle due sezioni successive verranno illustrate due tecniche che è possibile usare per difendere le applicazioni MVC ASP.NET da attacchi di attacchi JavaScript injection.

## <a name="approach-1-html-encode-in-the-view"></a>Approccio #1: codifica HTML nella visualizzazione

Un metodo semplice per impedire gli attacchi intrusivi JavaScript consiste nel codificare in HTML tutti i dati immessi dagli utenti del sito Web quando si rivisualizzano i dati in una vista. La vista `Index` aggiornata nel listato 3 segue questo approccio.

**Listato 3-`Index.aspx` (codificato in HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Si noti che il valore di `feedback.Message` è codificato in HTML prima che il valore venga visualizzato con il codice seguente:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Che cosa significa codificare in HTML una stringa? Quando si codifica in HTML una stringa, i caratteri pericolosi, ad esempio `<` e `>`, vengono sostituiti dai riferimenti alle entità HTML, ad esempio `&lt;` e `&gt;`. Quindi, quando la stringa `<script>alert("Boo!")</script>` è codificata in HTML, viene convertita in `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La stringa codificata non viene più eseguita come script JavaScript quando viene interpretata da un browser. Si ottiene invece la pagina innocua nella figura 3.

[![attacco JavaScript sconfitto](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: attacco JavaScript sconfitto ([fare clic per visualizzare l'immagine con dimensioni complete](preventing-javascript-injection-attacks-vb/_static/image9.png))

Si noti che nella visualizzazione `Index` in Listing 3 solo il valore di `feedback.Message` è codificato. Il valore di `feedback.EntryDate` non è codificato. È sufficiente codificare i dati immessi da un utente. Poiché il valore di EntryDate è stato generato nel controller, non è necessario codificare questo valore in HTML.

## <a name="approach-2-html-encode-in-the-controller"></a>Approccio #2: codifica HTML nel controller

Anziché i dati di codifica HTML quando si visualizzano i dati in una visualizzazione, è possibile codificare i dati in formato HTML immediatamente prima di inviare i dati al database. Questo secondo approccio viene adottato nel caso del `controller` nel listato 4.

**Listato 4-`HomeController.cs` (codificato in HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Si noti che il valore del messaggio è codificato in HTML prima che il valore venga inviato al database all'interno dell'azione `Create()`. Quando il messaggio viene visualizzato nuovamente nella visualizzazione, il messaggio è codificato in formato HTML e qualsiasi JavaScript inserito nel messaggio non viene eseguito.

In genere, è consigliabile preferire il primo approccio descritto in questa esercitazione rispetto a questo secondo approccio. Il problema con questo secondo approccio è che si finisce con dati codificati in HTML nel database. In altre parole, i dati del database sono sporchi di caratteri di aspetto buffi.

Perché questa operazione è negativa? Se è necessario visualizzare i dati del database in un elemento diverso da una pagina Web, si verificano problemi. Non è possibile, ad esempio, visualizzare più facilmente i dati in un Windows Forms Application.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è quello di spaventare l'utente sulla prospettiva di un attacco JavaScript injection. Questa esercitazione ha illustrato due approcci per la difesa delle applicazioni MVC ASP.NET da attacchi JavaScript injection: è possibile codificare in HTML i dati inviati dall'utente nella vista oppure è possibile codificare in HTML i dati inviati dall'utente nel controller.

> [!div class="step-by-step"]
> [Precedente](authenticating-users-with-windows-authentication-vb.md)
