---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper di Twitter con Pagine Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Questo argomento e l'applicazione illustrano come aggiungere un helper di Twitter al progetto WebMatrix 3. Contiene il codice dell'helper di Twitter e Mostra come chiamare l'helper...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638558"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Helper di Twitter con pagine Web ASP.NET

di [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Gli helper di Twitter sono obsoleti. Per gli strumenti di engagement più recenti di Twitter per siti Web, vedere la [Panoramica di Twitter per siti Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Questo argomento e l'applicazione illustrano come aggiungere un helper di Twitter al progetto WebMatrix 3. Contiene il codice helper di Twitter e Mostra come chiamare i metodi helper.
> 
> Questo codice per il file Twitter. cshtml è stato sviluppato da **Tian Pan** di Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

## <a name="introduction"></a>Introduzione

Questo argomento illustra come aggiungere un helper di Twitter all'applicazione e usare sintassi Razor per chiamare i metodi helper. L'helper di Twitter rende più semplice incorporare i pulsanti e i widget di Twitter nell'applicazione. Per usare un widget di Twitter, ad esempio la sequenza temporale di un utente o i risultati della ricerca per un hashtag, è necessario innanzitutto creare il [widget su Twitter](https://twitter.com/settings/widgets). Dopo aver creato il widget, si riceverà un ID widget. Questo ID widget viene passato come parametro quando si chiamano i metodi helper che visualizzano widget.

Questo argomento è stato scritto per la versione 1,1 dell'API Twitter. Aggiungendo direttamente il codice dell'helper Twitter al progetto, è possibile aggiornare il codice dell'helper se l'API Twitter cambia.

Per informazioni sull'installazione di WebMatrix, vedere [Introduzione a pagine Web ASP.NET 2 Introduzione](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Aggiungere l'helper di Twitter al progetto

Per aggiungere l'helper di Twitter, aggiungere prima di tutto una cartella denominata **App\_codice** al progetto. Quindi, creare un file denominato **Twitter. cshtml**.

![Cartella App_Code](twitter-helper/_static/image1.png)

Sostituire il codice predefinito in Twitter. cshtml con il codice seguente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chiamare i metodi di Twitter dalle pagine Web

Nell'esempio seguente viene illustrato come utilizzare i metodi helper di Twitter da una pagina del progetto. Nel progetto sarà necessario sostituire i valori dei parametri con i valori rilevanti per le proprie esigenze. È possibile usare gli ID widget forniti per esplorare il funzionamento dei metodi, ma sarà necessario generare widget personalizzati per il progetto.

Non tutti i parametri indicati di seguito sono obbligatori. I parametri facoltativi vengono usati per personalizzare il modo in cui viene visualizzato il pulsante o il widget. Il pulsante segue, ad esempio, richiede solo che il nome utente segua, ma nell'esempio viene illustrato come includere il numero di follower e come specificare le dimensioni del pulsante e della lingua.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Visualizzare i risultati

Il codice precedente produce i pulsanti e i widget seguenti. Questi pulsanti e widget sono completamente funzionanti e non schermate. Il pulsante segue viene visualizzato in spagnolo perché il parametro Language è stato impostato su **es**.

### <a name="follow-button"></a>Pulsante Segui

[Segui @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Pulsante Tweet

`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [Tweet](https://twitter.com/share)

### <a name="user-timeline-profile"></a>Sequenza temporale utente (profilo)

[Tweet per @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Preferiti

[Tweet preferiti per @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>List

[Tweet da @Microsoft/MS\_le bande di utenti\_](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Cerca

[Tweet su &quot;#asp&quot;.net](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
