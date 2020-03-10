---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: utilizzo delle annotazioni dei dati per la convalida del modello | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 6 illustra l'uso delle annotazioni dei dati per il modello V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539277"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: utilizzo delle annotazioni dei dati per la convalida del modello

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Nella Parte 6 viene illustrato l'utilizzo di annotazioni dei dati per la convalida del modello.

È stato riscontrato un problema importante con i moduli di creazione e modifica: non eseguono alcuna convalida. È possibile eseguire operazioni come lasciare vuoti i campi o digitare lettere nel campo Price e il primo errore che verrà visualizzato è il database.

È possibile aggiungere facilmente la convalida all'applicazione aggiungendo annotazioni dei dati alle classi del modello. Le annotazioni dei dati consentono di descrivere le regole che si desidera applicare alle proprietà del modello. ASP.NET MVC si occuperà di imporre tali regole e visualizzare i messaggi appropriati agli utenti.

## <a name="adding-validation-to-our-album-forms"></a>Aggiunta della convalida ai moduli degli album

Si useranno gli attributi di annotazione dei dati seguenti:

- **Obbligatorio** : indica che la proprietà è un campo obbligatorio
- **DisplayName** : definisce il testo che si vuole usare nei campi del modulo e nei messaggi di convalida
- **StringLength** : definisce una lunghezza massima per un campo stringa
- **Intervallo** : fornisce un valore massimo e minimo per un campo numerico
- **Binding** : elenca i campi da escludere o includere quando si associano valori di parametri o moduli alle proprietà del modello
- **ScaffoldColumn** : consente di nascondere i campi dai form dell'editor

*Nota: per ulteriori informazioni sulla convalida dei modelli mediante gli attributi di annotazione dei dati, vedere la documentazione MSDN all'* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Aprire la classe album e aggiungere le istruzioni *using* seguenti alla parte superiore.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Aggiornare quindi le proprietà per aggiungere gli attributi di visualizzazione e di convalida, come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Anche se siamo qui, abbiamo modificato il genere e l'artista in proprietà virtuali. Questo consente Entity Framework di caricarli in modo lazy se necessario.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Dopo aver aggiunto questi attributi al modello di album, la schermata di creazione e modifica inizia immediatamente a convalidare i campi e a usare i nomi visualizzati scelti, ad esempio l'URL dell'immagine dell'album anziché AlbumArtUrl. Eseguire l'applicazione e passare a/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Verranno quindi violate alcune regole di convalida. Immettere il prezzo 0 e lasciare vuoto il titolo. Quando si fa clic sul pulsante Create (Crea), verrà visualizzato il form con messaggi di errore di convalida che indicano quali campi non soddisfano le regole di convalida definite.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Test della convalida lato client

La convalida lato server è molto importante dal punto di vista dell'applicazione, perché gli utenti possono eludere la convalida lato client. Tuttavia, i form di pagine Web che implementano solo la convalida sul lato server presentano tre problemi significativi.

1. L'utente deve attendere che il modulo venga pubblicato, convalidato nel server e che la risposta venga inviata al browser.
2. L'utente non riceve feedback immediato quando corregge un campo in modo che ora passi le regole di convalida.
3. Le risorse del server vengono sprecate per eseguire la logica di convalida anziché utilizzare il browser dell'utente.

Fortunatamente, i modelli di impalcatura MVC 3 ASP.NET hanno una convalida lato client incorporata, che non richiede alcun lavoro aggiuntivo.

Digitare una singola lettera nel campo titolo soddisfa i requisiti di convalida, in modo che il messaggio di convalida venga rimosso immediatamente.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-5.md)
> [Successivo](mvc-music-store-part-7.md)
