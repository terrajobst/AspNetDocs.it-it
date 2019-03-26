---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Confronto tra visualizzazioni dinamiche Fortemente tipizzate viste | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: bde40f609db25f590108bfc2396071c0033a1326
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423338"
---
<a name="dynamic-v-strongly-typed-views"></a>Confronto tra visualizzazioni dinamiche e fortemente tipizzate
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

Esistono tre modi per passare le informazioni da un controller di visualizzazione ASP.NET MVC 3:

1. Come un oggetto modello fortemente tipizzato.
2. Come un tipo dinamico (con @model dinamica)
3. Usando ViewBag

Ho scritto un'applicazione MVC 3 Top Blog semplice per confrontare e contrapporre le visualizzazioni fortemente tipizzate e dinamiche. Il controller inizia con un semplice elenco di blog:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Fare clic con il pulsante destro del metodo IndexNotStonglyTyped() e aggiungere una visualizzazione Razor.

[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Assicurarsi che il **creare una visualizzazione fortemente tipizzata** non sia selezionata. La visualizzazione risultante non contenga molte:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Poiché utilizziamo un dinamico e non una visualizzazione fortemente tipizzata, non consentono di intellisense. Il codice completo è illustrato di seguito:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Ora si aggiungerà una visualizzazione fortemente tipizzata. Aggiungere il codice seguente al controller:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Si noti che è esattamente la stessa View(topBlogs) restituito; chiamare la visualizzazione non fortemente tipizzato. Fare clic con il pulsante destro all'interno di *StonglyTypedIndex()* e selezionare **Aggiungi visualizzazione**. Questa volta selezionare il **Blog** classe del modello e selezionare **elenco** come modello di scaffolding.

[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Supporto intellisense è disponibile all'interno del nuovo modello di visualizzazione.

[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Il progetto c# può essere scaricato [qui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
