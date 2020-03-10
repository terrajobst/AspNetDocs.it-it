---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: aggiornamenti finali per la navigazione e la progettazione del sito, conclusione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 10 illustra gli aggiornamenti finali per la navigazione e...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539368"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: aggiornamenti finali di esplorazione e progettazione del sito, conclusione

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 10 illustra gli aggiornamenti finali per la navigazione e la progettazione del sito, conclusione.

Sono state completate tutte le funzionalità principali per il sito, ma sono ancora disponibili alcune funzionalità da aggiungere alla navigazione nel sito, alla home page e alla pagina di esplorazione del negozio.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Creazione della visualizzazione parziale Riepilogo del carrello acquisti

Si vuole esporre il numero di elementi nel carrello acquisti dell'utente nell'intero sito.

![](mvc-music-store-part-10/_static/image1.png)

Questa operazione può essere implementata facilmente creando una visualizzazione parziale aggiunta al sito. master.

Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Per creare la visualizzazione parziale CartSummary, fare clic con il pulsante destro del mouse sulla cartella Views/ShoppingCart e scegliere Aggiungi visualizzazione. Assegnare alla visualizzazione il nome CartSummary e selezionare la casella di controllo "crea una visualizzazione parziale" come illustrato di seguito.

![](mvc-music-store-part-10/_static/image2.png)

La visualizzazione parziale CartSummary è molto semplice. si tratta semplicemente di un collegamento alla visualizzazione dell'indice ShoppingCart che mostra il numero di elementi presenti nel carrello. Il codice completo per CartSummary. cshtml è il seguente:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

È possibile includere una visualizzazione parziale in qualsiasi pagina del sito, incluso il master del sito, usando il metodo HTML. RenderAction. Per RenderAction è necessario specificare il nome dell'azione ("CartSummary") e il nome del controller ("ShoppingCart") come indicato di seguito.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Prima di aggiungerlo al layout del sito, verrà creato anche il menu genre, in modo da poter eseguire tutti gli aggiornamenti del sito. Master in una sola volta.

## <a name="creating-the-genre-menu-partial-view"></a>Creazione della visualizzazione parziale del menu genre

Possiamo semplificare molto più facilmente la navigazione degli utenti nell'archivio aggiungendo un menu di genere che elenca tutti i generi disponibili nello Store.

![](mvc-music-store-part-10/_static/image3.png)

Si seguiranno anche gli stessi passaggi per creare una visualizzazione parziale GenreMenu, che sarà quindi possibile aggiungere al master del sito. Aggiungere prima di tutto la seguente azione del controller GenreMenu a StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Questa azione restituisce un elenco di generi che verranno visualizzati dalla visualizzazione parziale, che verrà creata successivamente.

*Nota: è stato aggiunto l'attributo [ChildActionOnly] a questa azione del controller, che indica che si vuole usare questa azione solo da una visualizzazione parziale. Questo attributo impedisce l'esecuzione dell'azione del controller passando a/Store/GenreMenu. Questa operazione non è necessaria per le visualizzazioni parziali, ma è una procedura consigliata, poiché si vuole assicurarsi che le azioni del controller vengano usate come previsto. Si sta anche restituendo PartialView anziché View, che consente al motore di visualizzazione di tenere presente che non deve usare il layout per questa visualizzazione, perché è incluso in altre visualizzazioni.*

Fare clic con il pulsante destro del mouse sull'azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu che è fortemente tipizzata usando la classe di dati di genere View, come illustrato di seguito.

![](mvc-music-store-part-10/_static/image4.png)

Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu per visualizzare gli elementi usando un elenco non ordinato, come indicato di seguito.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aggiornamento del layout del sito per visualizzare le visualizzazioni parziali

È possibile aggiungere le visualizzazioni parziali al layout del sito (/Views/Shared/\_layout. cshtml) chiamando HTML. RenderAction (). Verranno aggiunti entrambi in, oltre ad alcuni markup aggiuntivi per visualizzarli, come illustrato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

A questo punto, quando si esegue l'applicazione, il genere verrà visualizzato nell'area di spostamento a sinistra e nel riepilogo del carrello nella parte superiore.

## <a name="update-to-the-store-browse-page"></a>Aggiornamento alla pagina di esplorazione del negozio

La pagina di esplorazione del negozio è funzionale, ma non ha un aspetto molto valido. È possibile aggiornare la pagina per visualizzare gli album in un layout migliore aggiornando il codice di visualizzazione (disponibile in/Views/Store/Browse.cshtml) come indicato di seguito:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Qui viene usato l'URL. Action anziché HTML. ActionLink in modo che sia possibile applicare una formattazione speciale al collegamento per includere la grafica dell'album.

*Nota: per questi album viene visualizzata una copertura di album generica. Queste informazioni vengono archiviate nel database di e sono modificabili tramite Gestione archivi. È possibile aggiungere un'immagine personalizzata.*

A questo punto, quando si passa a un genere, verranno visualizzati gli album mostrati in una griglia con la grafica dell'album.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aggiornamento della Home page per visualizzare gli album più venduti

Microsoft vuole avere a che fare con i nostri album più venduti sul home page per aumentare le vendite. Verranno apportati alcuni aggiornamenti ai HomeController per gestirli e aggiungere anche altri elementi grafici.

Prima di tutto, verrà aggiunta una proprietà di navigazione alla classe album in modo che EntityFramework sappia che sono associati. Le ultime righe della classe **album** dovrebbero ora essere simili alle seguenti:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: è necessario aggiungere un'istruzione using da inserire nello spazio dei nomi System. Collections. Generic.*

In primo luogo, verranno aggiunti un campo storeDB e MvcMusicStore. Models usando le istruzioni, come negli altri controller. Successivamente, verrà aggiunto il metodo seguente a HomeController che esegue una query sul database per trovare gli album più venduti in base a OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Si tratta di un metodo privato, perché non si vuole renderlo disponibile come azione del controller. Per semplicità, viene incluso nella HomeController, ma è consigliabile spostare la logica di business in classi di servizi separate, a seconda dei casi.

A questo punto, è possibile aggiornare l'azione del controller di indice per eseguire una query sui primi 5 album di vendita e restituirli alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Il codice completo per il HomeController aggiornato è come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Infine, sarà necessario aggiornare la visualizzazione dell'indice Home in modo che sia possibile visualizzare un elenco di album aggiornando il tipo di modello e aggiungendo l'elenco di album alla fine. Questa opportunità consente anche di aggiungere una sezione di intestazione e promozione alla pagina.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

A questo punto, quando si esegue l'applicazione, verranno visualizzati i home page aggiornati con i migliori album di vendita e il messaggio promozionale.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusione

Abbiamo visto che ASP.NET MVC semplifica la creazione di un sito Web sofisticato con accesso al database, appartenenza, AJAX e così via. abbastanza rapidamente. Speriamo che in questa esercitazione siano stati forniti gli strumenti necessari per iniziare a creare applicazioni MVC ASP.NET personalizzate.

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-9.md)
