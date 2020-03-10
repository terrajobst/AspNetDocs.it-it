---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Uso della classe TagBuilder per compilare helper HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther presenta una classe di utilità utile nel framework ASP.NET MVC denominato TagBuilder Class. È possibile usare la classe TagBuilder per semplificare...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b0aa9816209cc326d3dea4b8dfb1b13cf697fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600002"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Uso della classe TagBuilder per compilare helper HTML (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther presenta una classe di utilità utile nel framework ASP.NET MVC denominato TagBuilder Class. È possibile usare la classe TagBuilder per compilare facilmente i tag HTML.

Il framework ASP.NET MVC include una classe di utilità utile denominata classe TagBuilder che è possibile usare quando si compilano gli helper HTML. La classe TagBuilder, come suggerisce il nome della classe, consente di compilare facilmente i tag HTML. In questa breve esercitazione viene fornita una panoramica della classe TagBuilder e si apprenderà come usare questa classe quando si compila un helper HTML semplice che esegue il rendering dei tag HTML &lt;IMG&gt;.

## <a name="overview-of-the-tagbuilder-class"></a>Cenni preliminari sulla classe TagBuilder

La classe TagBuilder è contenuta nello spazio dei nomi System. Web. Mvc. Sono disponibili cinque metodi:

- AddCssClass (): consente di aggiungere un nuovo attributo *Class = ""* a un tag.
- GenerateId (): consente di aggiungere un attributo ID a un tag. Questo metodo sostituisce automaticamente i punti nell'ID (per impostazione predefinita, i punti vengono sostituiti da caratteri di sottolineatura)
- MergeAttribute (): consente di aggiungere attributi a un tag. Sono presenti più overload di questo metodo.
- SetInnerText (): consente di impostare il testo interno del tag. Il testo interno è codificato automaticamente in HTML.
- ToString (): consente di eseguire il rendering del tag. È possibile specificare se si vuole creare un tag normale, un tag di inizio, un tag di fine o un tag di chiusura automatica.

La classe TagBuilder dispone di quattro proprietà importanti:

- Attributes: rappresenta tutti gli attributi del tag.
- IdAttributeDotReplacement: rappresenta il carattere utilizzato dal metodo GenerateId () per sostituire i punti (il valore predefinito è un carattere di sottolineatura).
- InnerHTML: rappresenta il contenuto interno del tag. L'assegnazione di una stringa a questa proprietà *non* codifica in HTML la stringa.
- TagName: rappresenta il nome del tag.

Questi metodi e proprietà forniscono tutti i metodi e le proprietà di base necessari per creare un tag HTML. Non è necessario usare la classe TagBuilder. È invece possibile usare una classe StringBuilder. Tuttavia, la classe TagBuilder rende più semplice la vita.

## <a name="creating-an-image-html-helper"></a>Creazione di un helper HTML di immagine

Quando si crea un'istanza della classe TagBuilder, si passa il nome del tag che si vuole compilare al Costruttore TagBuilder. Successivamente, è possibile chiamare metodi come i metodi AddCssClass e MergeAttribute () per modificare gli attributi del tag. Infine, si chiama il metodo ToString () per eseguire il rendering del tag.

Il listato 1, ad esempio, contiene un helper HTML di immagine. L'helper di immagini viene implementato internamente con un TagBuilder che rappresenta un tag HTML &lt;IMG&gt;.

**Listato 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Il modulo nel listato 1 contiene due metodi di overload denominati Image (). Quando si chiama il metodo Image (), è possibile passare un oggetto che rappresenta un set di attributi HTML.

Si noti come il Metodo TagBuilder. MergeAttribute () venga usato per aggiungere singoli attributi, ad esempio l'attributo src, a TagBuilder. Si noti, inoltre, come viene usato il Metodo TagBuilder. MergeAttributes () per aggiungere una raccolta di attributi a TagBuilder. Il Metodo MergeAttributes () accetta un dizionario&lt;stringa&gt; parametro oggetto. La classe RouteValueDictionary viene utilizzata per convertire l'oggetto che rappresenta la raccolta di attributi in un dizionario&lt;stringa&gt;oggetto.

Dopo aver creato l'helper per le immagini, è possibile usare l'helper nelle visualizzazioni ASP.NET MVC esattamente come qualsiasi altro helper HTML standard. La visualizzazione nel listato 2 usa l'helper immagini per visualizzare la stessa immagine di una Xbox due volte (vedere la figura 1). L'helper Image () viene chiamato sia con sia senza una raccolta di attributi HTML.

**Listato 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]

[![finestra di dialogo nuovo progetto](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Figura 01**: uso dell'helper immagini ([fare clic per visualizzare l'immagine a dimensione intera](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))

Si noti che è necessario importare lo spazio dei nomi associato all'helper immagini nella parte superiore della vista index. aspx. L'helper viene importato con la direttiva seguente:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

In un'applicazione Visual Basic, lo spazio dei nomi predefinito corrisponde al nome dell'applicazione.

> [!div class="step-by-step"]
> [Precedente](creating-custom-html-helpers-vb.md)
> [Successivo](creating-page-layouts-with-view-master-pages-vb.md)
